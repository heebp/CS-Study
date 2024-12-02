TLB(Translation Lookaside Buffers)
=============

> TLB란?
  - 가상 메모리 주소를 물리 주소로 변환하는 속도를 높이기 위해 사용하는 캐시

> TLB를 왜 사용할까?
  - 운영체제에서 페이지 테이블에 접근하여 실제 메모리에 접근하는 방법으로 데이터를 얻기 때문에 여기서 발생하는 비용을 줄이고자 TLB 사용

> 동작되는 원리
  - TLB를 탐색
  - TLB에서 탐색이 완료되면 물리 주소로 접근(TLB 히트)
  - TLB에서 찾지 못하면 페이지 테이블로 접근(TLB 미스)
  - TLB 미스가 발생하면 MMU에서 페이지 테이블에 있는 정보를 TLB에 저장시킴
![가상메모리 이미지](/OperatingSystem/images/tlb.png)

### 참고 사이트
- https://velog.io/@becooq81/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-TLB
- https://velog.io/@orcasuit/Translation-Lookaside-Buffer-TLB
