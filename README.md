# 파일암호화
- 파일암호화를 한다.
- Java Cipher - 알고리즘, 운용 모드, 패딩의 이해 
    - http://happinessoncode.com/2019/04/06/java-cipher-algorithm-mode-padding/

## 파일을 암호화/복호화 할 알고리즘을 선택한다. ##
1. 복호화가 가능해야 되기 때문에 **양방향 암호화 알고리즘**을 사용한다.
2. 그 중에서도 속도를 고려한 대칭키 방식인 **AES**를 사용한다. DES방식은 결함이 발견되어 사용하지 않는다.
3. 더 자세한 내용은 https://github.com/orange601/Encrypt 참조한다.

## Block Cipher ##
- AES는 128 bit(16 byte)라는 고정된 블럭 단위로 암호화를 수행
- 키 값의 길이와는 전혀 무관
- 하지만 AES는 128 bit까지만 암호화 할 수 있으므로 128 bit가 넘어가는 데이터를 암호화하기 위해 Block Cipher Mode(EBC, CBC 등등)를 선택해야한다.
- 즉 256 bit의 평문을 암호화 할 때 두 개의 128 bit로 쪼개서 각각 암호화를 수행하게 된다.
- 또한 128 bit의 블럭으로 쪼개기 때문에 128 bit 보다 작은 블럭이 나올 수 있는데 이런 블럭은 뒤에 값을 붙여주는데 이 값을 padding이라고 부른다.
- 구현할때 사용하는 패딩의 종류를 보면 PKCS#5 padding을 사용하고 있다.
- https://perfectacle.github.io/2019/11/24/aes/ 참조

## 구현 순서 ##
1. 암호화, 복호화 기능을 제공하는 Cipher 클래스 객체 인스턴스화
    - AES 암호화, CBC operation mode ( 블록 암호의 운용 모드 ), PKCS5 padding scheme로 초기화
    ````java
    Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding");
    ````
2. SecretKey 생성
    ````java
    SecretKey secretKey = new SecretKeySpec("키로만들문자열".getBytes("UTF-8"), "AES");
    ````
    
3. Cipher 초기화(Initialization)
    ````java
    cipher.init(Cipher.ENCRYPT_MODE, SecretKey, IvParameterSpec);
    ````
    - **SecretKey와 IvParameterSpec Byte수가 같아야 한다.**
    - 블록 암호의 운용 모드가 CBC/OFB/CFB를 사용할 경우에는 Initialization Vector(IV), IvParameterSpec를 설정해줘야한다. 
    - 아니면 InvalidAlgorithmParameterException 발생
    - Operation Mode
        > **ENCRYPT_MODE**: cipher 객체를 암호화 모드로 초기화한다. 
        >
        > **DECRYPT_MODE**: cipher 객체를 복호화 모드로 초기화한다. 
        > 
        > **WRAP_MODE**: cipher 객체를 key-wrapping 모드로 초기화한다. 
        > 
        > **UNWRAP_MODE**: cipher 객체를  key-unwrapping 모드로 초기화한다.


