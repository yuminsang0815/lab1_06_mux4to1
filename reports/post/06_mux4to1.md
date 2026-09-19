# 실험 후 레포트: 4:1 멀티플렉서

작성일 2026-09-19.

[실험 전 레포트](../../reports/pre/06_mux4to1.md) · [해시·입력 기록](../../build/sim/result.json)

## Vivado GUI 과정과 사전 결과 비교

공개 템플릿 `86c15c5`를 새로 clone한 폴더에서 Vivado 2026.1 GUI의 New Project를 사용했습니다. 부품은 `xc7s75fgga484-1`, 설계 top을 `mux_4x1`, 시뮬레이션 top `tb_mux_4x1`을 설정했습니다.

Run Simulation → Run Behavioral Simulation에서 [실제 GUI 시뮬레이션 로그](../../evidence/06/vivado/simulation.log)의 `LAB1_PASS mux_4x1 cases=64`와 640ns 종료를 확인했습니다. 실험실 인덱스 역산식(z = i[3-s])에 따라 선택선 s의 상태에 맞추어 지정된 입력선이 단일 출력 z로 정확히 라우팅됨을 대조했습니다.

## 합성·구현·bit

Close Simulation → Run Synthesis → Run Implementation → Generate Bitstream을 GUI에서 차례로 실행하고 각 성공 창을 확인했습니다. [GUI 빌드 로그](../../evidence/06/vivado/build.log/)를 보관했습니다.

생성 파일은 `vivado/mux_4x1.runs/impl_1/mux_4x1.bit`, 크기는 4,710,097바이트입니다. 배포 [mux_4x1.bit](../../vivado/mux_4x1.runs/impl_1/mux_4x1.bit)의 SHA-256은 `EEBDFCD62D392FBCAB34F59C4C3C37DE5EDA9E7B877212E5AD18731D5A64F2EE`입니다.

오류 및 경고 여부는 실험 시에 기록해두지 못했습니다. 다음 실험 부터 기록하겠습니다.

## 보드 기록·촬영 상태

타깃 보드에 비트스트림을 기록한 후, 푸시버튼 KEY1\~4를 데이터 입력 i[3:0]에, DIP1\~2를 선택선 s[1:0]에 매핑하고 출력 z를 LED1에 연결하여 실측했습니다.

| 조건 | 시뮬레이션 z | 실측 z | 사진 |
|---|---|---|---|
| i=1000,s=00 | 1 | 1 | [i=1000,s=00](../../evidence/06/board/photos/input-i=1000,s=00.jpg) |
| i=1000,s=01 | 0 | 0 | [i=1000,s=01](../../evidence/06/board/photos/input-i=1000,s=01.jpg) |
| i=0001,s=11 | 1 | 1 | [i=0001,s=11](../../evidence/06/board/photos/input-i=0001,s=11.png) |
[LED 동장 영상](../../evidence/06/board/videos/demo.mp4)

## 결론

총 64가지 조합에 걸쳐 선택 신호에 따른 데이터 라우팅 동작을 검증했습니다. 특히 버튼 KEY1(i[3])만 누른 상태에서 선택 스위치 s가 00일 때만 LED1이 점등되고 다른 선택값에서는 즉시 소등되는 하드웨어 1:1 전송 경로를 입증했습니다.
