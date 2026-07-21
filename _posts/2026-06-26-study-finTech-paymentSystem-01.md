---
title: "카드 결제 프로세스"
description: 
author: nk
date: 2026-06-26 12:00:00 +0900
categories: [Study, FinTech, Card Payment]
tags: [Card Payment]
pin: false
image:
  path: /assets/img/posts/Study/FinTech/PaymentSystems/p1_1.png
---

카드를 긁으면 가게 주인은 언제 돈을 받게 되는지, 소비자가 어떤 카드사의 카드를 쓰든지 상관 없이 가게 주인은 어떻게 정산을 받게 되는지, 환불은 왜 며칠씩 걸리는지 궁금했었다.
그래서 카드 결제 시스템이 돌아가는 큰 틀과 전체적인 흐름을 공부해보았다.

<br><br>
### 결제 과정

![p1_2](/assets/img/posts/Study/FinTech/PaymentSystems/p1_2.png)

#### 구매자 → 판매자 → PG사 → 은행사/카드사

- PG (Payment Gateway)
    - 구매자와 판매자 사이에서 이루어지는 결제를 안전하게 할 수 있도록 대행해주는 역할
    - `KG 이니시스`, `NHN`, `KCP`, `LGU+`  / `KG 모빌리언스`, `다날`, `카카오Pay`

<br>

#### 구매자 → 판매자 → 결제 솔루션 → PG사 → 은행사/카드사

- 결제솔루션
    - 결제 API, 결제취소 API 등 결제 과정에 필요한 API 제공
    - PG사와의 연결 과정, 복잡한 결제 환경을 개발자가 직접 구현하지 않고 가져다 쓰면 됨
    - `iamport` `부트페이`

<br><br>
### 요청과 승인

1. 구매자 결제 정보, 최종 결제 금액(쿠폰, 적립금 적용 후의), 주문번호를 서버 세션이나 데이터베이스에 저장 
2. 결제 요청
3. 결제 인증
4. 승인할 데이터 검증: 요청하기 전 저장한 정보와 인증결과로 돌아온 정보가 같은지 검증
5. 결제 승인 요청
6. 결제 승인
7. 고객에게 상품이나 서비스가 제공됨

<br><br>
### 승인과 매입

![p1_3](/assets/img/posts/Study/FinTech/PaymentSystems/p1_3.png)

승인은 결제 순간에 실시간으로 처리되며, 한도와 카드 상태 등을 확인한 후 승인 여부를 결정한다. <br>
*이때 고객은 결제 완료 화면을 보게 되지만 실제 자금이 이동하는 것은 아님

반면 매입은 승인 이후에 정산 주기에 맞춰 배치 방식으로 처리된다.
고객에게는 이 과정이 보이지 않고, 가맹점이 카드사로부터 대금을 받을 수 있도록 실제 정산이 시작된다.

*가승인: 결제 수단의 유효성이나 잔액을 확인하기 위해 최종 결제 전에 미리 한도를 잡아두는 임시 가결제 상태

<br><br>
### PG사와 VPN

![p1_4](/assets/img/posts/Study/FinTech/PaymentSystems/p1_4.png)

PG와 VAN은 가맹점과 카드사 사이의 중계 역할을 한다.

금융기관과 직접 계약하기 어려운 온라인 쇼핑몰을 대신하여, 금융기관 (카드사, 은행) 과 PG사가 계약을 맺고 발생된 결제대금을 대신 지급 받아 가맹점에게 일정 수수료를 받고 지급해주는 온라인 서비스이다.

<br><br>
![p1_5](/assets/img/posts/Study/FinTech/PaymentSystems/p1_5.png)

VAN은 카드사와 상점의 통신을 연결하는 부가가치통신망이다.
데이터를 전송하는 네트워크로 볼 수 있다.

오프라인 상점에서 입력한 고객의 결제 데이터를 카드사로 안전하게 보내주는 역할

오프라인에서는 POS 단말기에서 발생한 결제 정보를 VAN이 카드사로 전달하고, 카드사의 승인 결과를 다시 가맹점에 전달한다. 
즉, VAN은 카드사와 가맹점을 연결하는 통신망으로 승인 정보를 안전하게 중계하는 역할

<br><br>
### 신용카드 취소 
⇒ 매입 시점 기준

![p1_6](/assets/img/posts/Study/FinTech/PaymentSystems/p1_6.png)

카드 대금 청구 전 취소 ⇒ 청구 금액에서 취소분 차감
카드 대금 청구 후 취소 ⇒ 다음 청구에서 취소분 차감

- 승인취소: 당일 자정(24시) 전까지 취소한 경우 바로 취소 처리
- 매입취소: 자정 이후 취소한 경우 ⇒ 이미 정산 프로세스에 들어감 → **정산 취소 → 환불**
- 부분취소: 부분취소는 당일에 요청하더라도 전체 금액을 매입 처리한 후 부분취소를 진행

<br>
**취소 원복**

- 취소를 취소하는 것
- 취소 접수 당일에만 원복이 가능

<br><br><br>
참고

[https://www.tosspayments.com/blog/articles/33907](https://www.tosspayments.com/blog/articles/33907)<br>
[https://developers.hectofinancial.co.kr/blog/payment-cancel-refund-guide](https://developers.hectofinancial.co.kr/blog/payment-cancel-refund-guide)<br>
[https://docs.tosspayments.com/resources/glossary/card-payment](https://docs.tosspayments.com/resources/glossary/card-payment)