# 우분투, Bash 및 쉘 스크립트

## 우분투 기본쉘을 dash에서 bash로 바꾸기

리눅스 스크립트를 종종 실행하는데 bash용으로 작성된 스크립트가 많다고 한다. dash 쉘에서 실행하면 오류가 발생할 수 있기 때문에 우분투의 기본쉘인 dash를 bash로 전환할 필요가 있다.

```txt
ls -al /bin/sh
sudo dpkg-reconfigure dash
```

## Bash 쉘 스크립트 작성

### 콘솔에 문자열 출력

```bash
echo "문자열"
printf "Hello bash world"
printf "%s %s %s" Hello bash world.
```

### 주석

'#' 기호를 이용해서 주석을 달 수 있다.

```bash
# 샾을 입력하여 주석을 달 수 있다.
```

### 변수

#### 변수 선언

변수 식별자와 값 사이에 = 를 공백없이 입력한다.

```bash
myVariable=20
```

기본적으로 bash 스크립트에서 선언되는 변수는 전역변수이다. 만약 특정 함수 내에서만 유효한 지역변수를 만들고 싶다면, local 키워드를 추가하면 된다.

```bash
mystring="hello bash"
echo $mystring
echo ${mystring}

my_test() {
    local local_string="local"
    echo $local_string
}

my_test # 함수 호출
```

전역적으로 선언한 변수는 현재 스크립트에서만 사용 가능하고, 자식 스크립트에서는 사용 불가능하다. 만약 자식 스크립트에서도 사용가능하게 하려면 export 키워드를 추가하면 된다.

```bash
export hello_world="my bash script"
```

#### 변수 참조

$기호 옆에 변수 이름을 넣거나 또는 $기호와 함께 중괄호({})로 변수 이름을 둘러싸면 된다.

```bash
echo $myVariable

echo ${myVariable}
```

### 함수

function 키워드를 쓰거나 생략 가능하다. 함수명을 쓰면 함수를 호출할 수 있게된다. 함수의 정의가 함수 호출보다 먼저 위치해야한다.

```bash
my_test1() {
    echo "string test"
}

function my_test2() {
    echo "string test 2"
    echo "인자값: ${@}"
}

my_test1
my_test2
my_test2 "hello" "world"
```

#### 위치 매개변수

- $0 : 실행된 스크립트 이름
- $1, $2, $3, $4, ... , ${10} : 인자 순서대로 번호가 부여됨. 10번 부터는 중괄호로 감싸줘야함.
- $* : 전체 인자값
- $@ : 전체 인자값. 위와 동일하나 따옴표로 감싸는 경우 차이가 있음
- $# : 매개변수 총 개수

### 배열 선언

```bash
declare -a arrayName

arrayName=(10, 20, 30)

# 배열에 값 추가하기
arrayName[3]=40

# 또다른 방법
arrayName=(${arrayName[@]}, 50)

# 배열 전체 출력
printf "배열 출력: ${arrayName[@]}"

# 전체 배열 개수 출력
printf "배열 개수: ${#arrayName[@]}"

# 배열 원소를 순회하면서 전체 출력
printf "배열 원소: %s\n" ${arrayName[@]}

# 배열값 제거
unset arrayName[4]

# 전체 배열 제거
unset arrayName
```

### 변수 타입 지정

bash 스크립트는 기본적으로 변수 값을 문자열로 처리한다. 그러나 10, 20 과 같은 숫자는 자동으로 숫자로 처리할 수도 있다. 그러나 언제 어떻게 자동으로 처리될지 모르기 때문에 변수의 타입을 지정해주어 확실하게 타입을 보장하는 것이 좋다.

```bash
# 변수를 읽기 전용 타입으로 선언
declare -r myVariable

# 정수 타입으로 변수 선언
declare -i myVariable=10

# 배열 선언
declare -a arrayName

# 환경 변수로 선언. export export_variable="hello bash"와 동일
declare -x export_variable="hello bash"

# 현재 스크립트의 전체 함수 출력하고 싶을 때 사용하는 명령
declare -f

# 현재 스크립트에서 지정한 함수만 출력하고 싶을 때 사용하는 명령
declare -f 함수이름
```

### 반복문

#### for 반복문

```bash
for string in "hello" "bash" "world"; do;
    echo ${string}
done;
```

#### while 반복문

```bash
count1=0
while [ ${count1} -le 3 ]; do
    echo ${count1}
    count=$((${count} + 1))
done
```

#### until 반복문

```bash
count2=10
until [ ${count2} -le 3]; do
    echo ${count2}
    count=$((${count2} - 1))
done
```

### 조건문

조건문 작성시 주의할 점은 조건문 안쪽에 실행문이 없으면 오류가 나타난다는 것이다.

#### if 조건문

```bash
if [조건]; then

fi

if [ ${mystring1} == ${mystring2} ]; then
    echo "hello world"
elif [ ${mystring1} == ${mystring3} ]; then
    echo "hello bash same"
else
    echo "hello bash different"
fi

if [ ${mystring1} == ${mystring2} ] && [${mystring3} == ${mystring4}]; then
    ...
fi

if [[ ${mystring1} == ${mystring3} ] || [${mystring3} == ${mystring4}]] && [${mystring5} == ${mystring6}]; then
    ...
```

### 선택문

정규표현식을 지원한다. case문 작성시 반드시 문장 끝을 ';;'로 작성한다.

```bash
for string in "HELLO" "BASH" "hello" "bash" "star" "start" "end" "finish"; do
    case ${string} in
        hello|HELLO)
            echo "${string}: hello일 때 실행하는 문장이다."
            ;;
        hard*)
            echo "${string}: hard로 시작하는 단어일 때 실행하는 문장이다."
            ;;
        star|start)
            echo "${string}: star 혹은 start일 때 실행하는 문장이다."
            ;;
        finish|end)
            echo "${string}: finish 혹은 end일 때 실행하는 문장이다."
            ;;
        *)
            echo "${string}: 기타"
            ;;
    esac
done
```

## 콘솔에서 쉘 스크립트 실행

```bash
bash ./shell_script.sh
```
