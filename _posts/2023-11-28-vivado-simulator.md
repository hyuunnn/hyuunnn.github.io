---
layout: post
title: "Vivado 시뮬레이터 사용하기"
description: ""
date: 2023-11-28
tags: []
---

학교 수업에서 Vivado를 사용하고 있는데 작성한 Verilog 코드의 동작을 확인하려면 보드에 올리는 과정에서 시간이 많이 소요된다.

시뮬레이터가 있지 않을까 찾아보다가 우연치 않게 <a href="https://youtu.be/4qwAEJ_3-8o">설계독학 - Hello World 프로그램</a> 유튜브 영상에서 Vivado Simulator라는 문구와 함께 CLI 환경에서 사용하는 것을 확인했다.

그러나 영상에서는 `./build`로 결과를 확인하는데 해당 강의를 구매해야 예제 파일들을 받을 수 있었다.

인프런 질문 글(<a href="https://www.inflearn.com/questions/450178/build%EC%97%90-%EA%B4%80%ED%95%B4%EC%84%9C">1</a>, <a href="https://www.inflearn.com/questions/752579/vivado-%EC%8B%9C%EB%AE%AC%EB%A0%88%EC%9D%B4%EC%85%98-%EB%94%94%EB%B2%84%EA%B9%85-%EA%B4%80%EB%A0%A8-%EC%A7%88%EB%AC%B8">2</a>)에서 `build` 파일의 내용을 확인할 수 있었고 해당 파일에서는 `xvlog`, `xelab`, `xsim` 명령어를 사용하고 있었다.

Verilog 파일명은 `hello.v` 라고 가정한다.

```verilog
module test;
initial begin
$display("Hello world");
$finish;
end
endmodule
```

## xvlog

```console
The xvlog command is the Verilog parser. The xvlog command parses the Verilog source file(s) and stores the parsed dump into a HDL library on disk.
```

`xvlog` 명령어는 Verilog 코드를 HDL 라이브러리 형태로 생성한다.

```console
$ xvlog hello.v
```

![0](/assets/images/vivado-simulator/0.png)

위의 명령어를 수행하면 `xsim.dir/work`라는 폴더가 생성되고 그 안에 모듈명으로 `sdb` 파일이 저장된다.

## xelab

`xelab` 명령어는 HDL 라이브러리를 기반으로 시뮬레이션 스냅샷을 생성한다. 이때 위에서 생성된 `sdb` 파일을 사용한다.

`-R` 옵션을 사용하면 커맨드 창에서 출력 결과를 확인할 수 있다.

```console
$ xelab test -R
```

위 명령어에서 `test`는 모듈명을 의미한다. Verilog 코드에서 모듈명을 확인하자.

![0](/assets/images/vivado-simulator/4.png)

`-debug wave` 옵션을 사용하면 Waveform을 사용할 수 있는 기능을 제공한다.

`-debug line` 옵션을 사용하면 HDL breakpoint를 설정할 수 있다. 그 외에 다양한 옵션들은 `xelab --help`를 참고한다.

`-s` 옵션을 사용하면 스냅샷 파일이 생성되는 폴더명을 지정할 수 있다.

```console
$ xelab test -debug wave -s hello_sim
```

![0](/assets/images/vivado-simulator/1.png)

위의 명령어를 수행하면 `xsim.dir/hello_sim`이라는 폴더가 생성되고 그 안에 다양한 파일들이 생성된다. `-s` 옵션을 사용하지 않으면 `xsim.dir/work.test` 폴더에 생성된다.

![0](/assets/images/vivado-simulator/2.png)

`sdb` 파일을 삭제한 결과 위와 같이 에러가 발생하는데 `xelab` 명령어를 수행할 때 필요한 파일임을 확인할 수 있다.

## xsim

`xsim` (XSim simulation executable) 명령어는 `xelab` 명령어를 통해 생성된 파일을 기반으로 시뮬레이션을 수행한다.

```console
xsim hello_sim -gui -wdb hello_waveform.wdb
```

![0](/assets/images/vivado-simulator/3.png)

`-gui` 옵션을 사용하면 Vivado GUI 프로그램이 실행되고 `-wdb` 옵션을 사용하면 생성되는 WDB(Waveform Database file) 파일의 이름을 지정할 수 있다. 위 옵션이 없으면 `hello_sim.wdb` 파일로 생성된다.

위 코드는 단순히 출력하는 파일이라서 input, output이 없지만 input, output이 있는 경우 아래 사진처럼 Objects에서 확인할 수 있다.

![0](/assets/images/vivado-simulator/5.png)

이때 `Add to Wave Window`를 클릭하면 Waveform 창에 추가할 수 있다.

## 테스트벤치

<a href="https://verilog-hdl-design.tistory.com/14">테스트벤치 작성 예시</a>

<a href="https://youtu.be/sGQoBnFcmwc">Writing a Verilog Testbench</a>

<a href="https://youtu.be/onMmG_U4SVo">How to use vivado for Beginners | Verilog code | Testbench | Schematic View</a>

<a href="https://youtu.be/hK6vBKjPs-k">Verilog Code for D Flip Flop with Testbench | Sequential Circuits | Vivado Simulator</a>

<a href="https://youtu.be/-Kdbzax9EOQ">Vivado Simulator and Test Bench in Verilog</a>

Quartus에서는 마우스로 Waveform을 그려서 사용했었는데 Vivado에서는 테스트벤치 파일을 작성해야하는 것 같다.

## 참고자료

<a href="https://itsembedded.com/dhd/vivado_sim_1/">Series on Vivado Simulator Scripted Flow</a>

<a href="https://yhkwon6658.github.io/2023-01-29/systemverilog-compiler-and-simulator">Window 에서 사용 가능한 System Verilog 컴파일러와 시뮬레이터</a>

<a href="https://docs.xilinx.com/r/en-US/ug900-vivado-logic-simulation/xelab-xvhdl-and-xvlog-xsim-Command-Options">xilinx docs</a>

<a href="https://xilinx.github.io/xup_fpga_vivado_flow/lab1.html">Vivado Design Flow</a>
