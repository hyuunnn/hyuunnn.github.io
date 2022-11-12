---
layout: post
title: "Powershell 언어의 특이한 기능"
description: ""
date: 2022-11-12
tags: ["Powershell"]
---

학교 과제로 rare한 언어의 튜토리얼을 만드는게 있어서 Powershell을 선택하였다. (해당 언어는 현장에서 사용되고 있어야 한다.)

Powershell은 윈도우에 내장되어 있기 때문에 악성코드에서 자주 사용되며, 포렌식 분야에서도 종종 보이고 있다. (<a href="https://github.com/kacos2000/WindowsTimeline">WindowsTimeline</a>, <a href="https://github.com/Yamato-Security/WELA">WELA</a>)

예전부터 공부해보고 싶다는 생각을 가지고 있었는데, 미루고 미루다가 학교 과제로 선택하여 공부를 시작했다.

좋은 퀄리티의 과제물을 만들고 싶어서 온라인 튜토리얼 외에도 책도 빌려서 공부를 진행했고, 다른 언어에 비해서 특이하다?고 볼 수 있는 기능들이 몇 가지 있어서 정리하려고 한다. (아직 다양한 언어를 접해보지 않아서 특이하다고 생각하는 것일 수도...)

### return 값이 있는 코드는 모두 출력이 된다는 것이다.

```powershell
function f1
{
    $a = New-Object System.Collections.ArrayList
    $a.Add(1)
    $a.Add(2)
    return $a
}

$ret1 = f1
$ret1.getType().fullname
Write-Host $ret1
# System.Object[]
# 0 1 1 2

function f2
{
    $a = New-Object System.Collections.ArrayList
    [void]$a.Add(1)
    [void]$a.Add(2)
    return $a
}

$ret2 = f2
$ret2.getType().fullname
Write-Host $ret2
# System.Object[]
# 1 2

function f3
{
    $a = New-Object System.Collections.ArrayList
    [void]$a.Add(1)
    [void]$a.Add(2)
    $a
}

$ret3 = f3
$ret3.getType().fullname
Write-Host $ret3
# System.Object[]
# 1 2
```

f1 함수를 실행했을 때 0 1 1 2가 나오는데, `$a.Add(1)`과 `$a.Add(2)`를 했을 때 return 값으로 나오는 index 값이 출력되었기 때문이다. 이를 해결하기 위해서는 `[void]`를 추가하여 return 값이 없다는 것을 명시해줘야 한다.

이는 f3 함수처럼 `return $a` 말고도 `$a`만 적어서 값을 반환하는 코드를 작성할 수도 있다. 

### 괄호는 파라미터에 들어갈 값을 구분한다.

```powershell
# 개발자를 위한 파워셸 - Douglas Finke p38
function Test ($p, $x) {
    "This is the value of p $p and the value of x $x"
}
Test (1, 2)
# This is the value of p 1 2 and the value of x

Test 1 2
# This is the value of p 1 and the value of x 2
```

위 결과를 보면 첫 번째 방법은 $p에 1, 2가 모두 들어가있는 것을 확인할 수 있다.

괄호를 기준으로 구분하기 때문에 두 번째 방법을 사용해야 한다.

### cmdlet 명령 결과를 변수에 저장할 수 있다.

```powershell
# 개발자를 위한 파워셸 - Douglas Finke p37
$animals = "cat", "dog", "bat"
$n = $animals -ne 'cat'
$n.GetType()
Write-Host $n

$contain_a_value = $animals -like "*a*"
$contain_a_value.getType()
Write-Host $contain_a_value

$a = $animals | Get-Member
$a.getType().fullname
$a[0]

$b = $animals | Get-Member -MemberType Property
$b.getType().fullname
$b[0]

# IsPublic IsSerial Name       BaseType
# -------- -------- ----       --------
# True     True     Object[]   System.Array
# dog bat
# True     True     Object[]   System.Array
# cat bat
# System.Object[]

# TypeName   : System.String
# Name       : Clone
# MemberType : Method
# Definition : System.Object Clone(), System.Object ICloneable.Clone()

# Microsoft.PowerShell.Commands.MemberDefinition

# TypeName   : System.String
# Name       : Length
# MemberType : Property
# Definition : int Length {get;}
```

원하는 결과가 나오는 cmdlet 명령어가 있다면 이를 변수에 저장하여 활용할 수 있고, 옵션을 사용하여 필터링한 결과를 저장할 수도 있어서 활용도가 높다는 것을 알 수 있었다.

변수에는 Object가 저장되며, Get-Member를 사용하면 구현되어 있는 메서드, 프로퍼티 정보들을 확인할 수 있다.

### 함수의 파라미터를 param이라는 키워드를 사용해서 선언할 수 있다.

```powershell
function plusNumber1 ([Int]$a, [Int]$b) 
{
    return $a + $b
}

function plusNumber2 {
    param([Int]$a, [Int]$b)
    return $a + $b
}

plusNumber1 1 2
# 3
plusNumber2 1 2
# 3
```

첫 번째는 일반적인 언어에서 사용하는 방법이며, param이라는 키워드를 사용하여 파라미터에 들어갈 값을 정의할 수 있다. 두 함수는 똑같은 결과가 출력된다.

### 더 공부해보기

<a href="http://www.yes24.com/Product/Goods/12759997">개발자를 위한 파워셸</a>

<a href="http://www.yes24.com/Product/Goods/59058752">실무에서 바로 쓰는 파워셸</a>

첫 번째 책은 2012년도에 출간된 책이라서 오래되었지만, 예제 코드가 일부 동작이 안되는 경우를 제외하고는 문제 없이 읽을 수 있었다. 

단순한 명령어, 언어 설명이 아닌 상황을 가정하고 실습해보는게 재미있었다. 과제 마감 시간 때문에 정독은 못했는데, 종강하고 다시 읽어봐야겠다. 



두 번째 책은 한국에서 유일하게 작성된 책이다보니 읽어봐야겠다는 생각을 가지고 있다.

<a href="https://github.com/PoshCode/PowerShellPracticeAndStyle">PowershellPracticeAndStyle</a> Powershell 코드 스타일과 관련 예제들이 있다.

<a href="https://learn.microsoft.com/ko-kr/powershell/">Microsoft Powershell 설명서</a> Microsoft 공식 사이트에 있는 Powershell 튜토리얼이다.

### 앞으로 계획 중인 아이디어?

Windows Debloat라는 단어로 불필요한 프로그램들을 삭제하는 Powershell 프로젝트가 많은데, 나만의 윈도우 세팅을 해주는 프로그램은 못본 것 같다. 

파일 확장자 보이게 하기, 최근 실행 파일 안보이게 하기, wsl 활성화, rdp 설정 등 윈도우 포맷을 할 때마다 가이드를 찾아서 해야 한다.

사용자만의 json, xml 파일을 활용하여 자동화하는 프로그램이 있으면 좋겠다는 생각이 들었다.

방학 때 CUI, GUI로 만들어보면 어떨까 생각을 가지고 있다.