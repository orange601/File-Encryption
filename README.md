# 파일암호화
- 파일암호화를 한다.

## 파일을 암호화/복호화 할 알고리즘을 선택한다. ##
1. 복호화가 가능해야 되기 때문에 **양방향 암호화 알고리즘**을 사용한다.
2. 그 중에서도 속도를 고려한 대칭키 방식인 **AES**를 사용한다. DES방식은 결함이 발견되어 사용하지 않는다.
3. 더 자세한 내용은 https://github.com/orange601/Encrypt 참조한다.

## 구현 순서 ##
  1. 암호화, 복호화 기능을 제공하는 Cipher 클래스 객체 인스턴스화
      - AES 암호화, CBC operation mode, PKCS5 padding scheme로 초기화
      ````java
      Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding");
      ````
  
