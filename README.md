---
description: 这是一份GTEA控制文件的开发与重构指南，旨在协助开发者将CTL文件格式转为JSON文件格式。
---

# GTEA控制文件编写指南

## 为什么使用JSON？

* 结构化控制文件，改善通用性与可扩展性；语义化部分控制参数，提高其可读性、

{% tabs %}
{% tab title="JSON" %}
{% code-tabs %}
{% code-tabs-item title="case.json" %}
```javascript
/*
createdDate:  2018-12-24T03:24:00
modifiedDate: 2018-12-25T11:24:00
*/
{
    "gridInfo": [
        {
            "fileName": "case.cgns",
            "settings": 
            [
                {"name": "air",    "type": "acoustic"},
                {"name": "porous", "type": "acoustic"}
            ]
        }
    ],
    "additionalGridInfo": {
        "space": "3D",
        /* Space type, one of "AXI2D", "3D", 
        "2D", "REVOLVE" */
        "scale": 1.0,
        "numPhases": 1,
        "homogeneous": true
    } 
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}

{% tab title="CTL" %}
{% code-tabs %}
{% code-tabs-item title="case.ctl" %}
```d
Ring case
DUMMY2
**The following section for phase info for each zone**
NREC=1 NUM OF CGNS FILES
 1
NREC=1 CGNS FILE NAME
 case.cgns 2
NREC=1 NUMBER OF ZONES
 2 1
 6 6
NREC=1 AXI2D 3D 2D REVOLVE
 F T F F
NREC=1 NUMBER OF PHASES  homogeneous (yes/no)
 1 1
NREC=1 GRID SCALE
 1.0
END
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}

显然，这里JSON文件的提供的信息相较于CTL文件而言先得更加丰富，精确。

* 简化GTEA输入，改善GTEA控制文件信息读入部分程序的可读性。

以网格信息为例，展示JSON解析器与CTL解析器的区别。

{% tabs %}
{% tab title="JSON parser" %}
{% code-tabs %}
{% code-tabs-item title="json\_parser.f90" %}
```css
program input
    use fson
    use fson_value_m, only: fson_value_count, fson_value_get
    implicit none
    
    character,pointer,dimension(:) :: cgns_file_name
    integer,pointer,dimension(:) :: zone_type
    integer :: nphase,trasient
    real :: scale
    logical :: is_homogeneous, is_AXI2D, is_3D, is_2D, is_REVOLVE
    type zone_information
        character :: zone_name
        integer :: problem_type /*1=流体，2=结构，6=声学*/
    end type
    type(zone_information),dimension(:) :: zone
    
    integer :: i,j,n_cgns,n_zone
    character :: zone_type，spcae_type
    
    
    type(fson_value), pointer :: json_data, l1_array, l2_array, item
    
    /***只用下面一行就能将json中所有信息整理完毕****/
    json_data => fson_parse ("case.json")
    /*****************************************/
    /*接下来只要将json文件信息与控制变量对号入座就行*/
    /*****************************************/
    
    /*PART1：这是唯一一个比较复杂的情况，因为涉及到了二维数组的输入*/
    /*计算输入网格文件的数量（数组大小，用于分配内存）*/ 
    call fson_get(json_data, "gridInfo", l1_array)
    n_cgns=fson_value_count(l1_array)
    /*根据控制文件提供的网格数量，分配网格文件名数组内存*/
    allocate(cgns_file_name(n_cgns))
    do i=1, fson_value_count(l1_array)
        /*循环，对每个网格文件的控制信息*/
        item => fson_array_get(l1_array,i)
        /*读取网格文件名*/
        call fson_get(item, "fileName", cgns_file_name(i))
        
        /*计算所有网格文件的zone的个数*/
        n_zone=0
        call fson_get(item, "settings", l2_array)
        n_zone=n_zone+fson_value_count(l2_array)
    enddo
    /*分配zone_information的内存*/
    allocate(zone(n_zone))
    
    /*读入zone_informnation*/
    n_zone=0
    do i=1, n_cgns
        item => fson_array_get(l1_array,i)
        call fson_get(item, "settings", l2_array)
        do j=1, fson_value_count(l2_array)
            n_zone=n_zone+1
            call fson_get(item, "name", zone(n_zone)%name)
            call fson_get(item, "type", zone_type)
            selectcase(zone_type)
            case("acoustic")
                zone(n_zone)%problem_type=6
            case("fluid")
                zone(n_zone)%problem_type=1
            case("solid")
                zone(n_zone)%problem_type=2
            case default
                zone(n_zone)%problem_type=1
            endselect
        enddo
    enddo
    
    /*PART2：读入附加网格信息*/
    call fson_get(json_data, "additionalGridInfo.space", space_type)
    selectcase(space_type)
    case("AXI2D")
        is_AXI2D= .true.
        is_3D= .false. ; is_2D= .false. ; is_REVOLVE= .false.
    case("3D")
        is_3D= .true.
        is_AXI2D= .false. ; is_2D= .false. ; is_REVOLVE= .false.
    case("2D")
        is_2D= .true.
        is_3D= .false. ; is_AXI2D= .false. ; is_REVOLVE= .false.
    case("")
        is_REVOLVE= .true.
        is_3D= .false. ; is_2D= .false. ; is_AXI2D= .false.
    case default
        is_3D= .true.
        is_AXI2D= .false. ; is_2D= .false. ; is_REVOLVE= .false.
    endselect
    
    call fson_get(json_data, "additionalGridInfo.scale", scale)
    call fson_get(json_data, "additionalGridInfo.space", space_type)
    call fson_get(json_data, "additionalGridInfo.numPhases", nphase)
    call fson_get(json_data, "additionalGridInfo.homogeneous", is_homogeneous)
    
end program input
    
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}

{% tab title="CTL parser" %}
{% code-tabs %}
{% code-tabs-item title="ctl\_parser.f90" %}
```css



```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}

* 为GTEA的界面开发提供良好的接口环境

使用CTL格式编写控制文件意味着GTEA界面软件开发者需要额外编写一套CTL格式的解析器才能正常调用GTEA。若使用JSON格式编写控制文件则省去了这一过程，因为当前[众多编程语言](https://www.json.org/)都有完整的JSON的解析器。

* 更加自由的控制文件编写方式（更好的鲁棒性）

目前CTL格式有序（顺序读写），而JSON无序的数据结构使得控制文件的编写更加自由可靠。不会因为多写了一行还是少写了一行就导致程序崩溃。另外，当使用者写了一个数据不全的控制文件给GTEA，GTEA-fson会告诉使用者GTEA还需要什么数据，该数组应该被放在什么位置。

* 可作为GTEA的使用教程

## JSON看起来挺复杂的，会不会反倒不方便用户使用？

JSON作为通用数据交换语言具有轻量、简单、可扩展、交互性强、开放等特点，不论是手动编写还是开发相应程序编写都十分方便。通常，用户直接与界面程序交互，界面程序将控制数据以控制文件z作为载体传递给GTEA；当然GTEA可以实现的功能并不一定完全在界面程序上有所体现，所以用户也可以选择直接编写控制文件。

![&#x4F7F;&#x7528;json&#x65F6;GTEA&#x7684;&#x63A7;&#x5236;&#x6570;&#x636E;&#x8BFB;&#x5165;&#x65B9;&#x5F0F;](.gitbook/assets/image%20%281%29.png)

另外，网上不乏在线json语法检查器：[jsonvalidator](https://codebeautify.org/jsonvalidator)、[bejson](https://www.bejson.com/)、[json.cn](https://www.json.cn/)、[kjson](http://www.kjson.com/)等，用户无需为自己编写控制文件时的笔误担心。

