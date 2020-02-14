# RPI에 웹개발 관련 소프트웨어 설치 기록

## Apache2

## php, 파이썬 설치 및 cgi 설정

34에서 38페이지에 걸쳐서 php, 파이썬으로 cgi를 이용할 수 있도록 설정하고 있다. 렌더링 되는 각 파일이 저장되는 위치를 주목하자.

## phpMyadmin 설치시 오류

phpMyAdmin을 라즈비안에 설치 후 웹브라우저를 통해 접속해보았으나

```txt
mysqli 확장기능이 설치되지 않았습니다. PHP의 설정을 확인하십시오. See our documentation for more information.
```

위와 같은 오류메시지가 나타났다. 설치 과정에서 문제가 있었나 싶어 라즈비안을 재설치했지만 해결되지 않았다(좌절).

/etc/php/7.3/apache2/php.ini

파일에 extension 관련 부분을 적절히 설정한다. [참고](https://hungry2s.tistory.com/182)

### extension 라이브러리 위치설정

(754라인 근처)

mbstring.so, mysqli.so 파일이 있는 위치를 지정해주어야 한다. 나의 경우에는

/usr/lib/php/20180731

이라는 폴더이 있었다.

### extension 사용 설정

(881라인 근처)

mbstring과 mysqli 관련 확장을 사용할 수 있도록 앞쪽의 세미콜론(;)를 제거하여 사용할 수 있도록 했다.
