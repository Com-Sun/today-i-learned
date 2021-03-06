# 포인터 씹어먹기

이 문서는 *comp.lang.c*의 공식 [FAQ](http://www.c-faq.com/index.html)와, 이를 번역하신 신성국님의 [온라인 문서](https://cinsk.github.io//ko/cfaqs/index.html)를 참고하였습니다.

모든 내용은 C99에 해당하는 레퍼런스를 가집니다.

# 배열과 포인터

    A strength of C is its unification of arrays and pointers. 
    Pointers can be conveniently used to access arrays and to simulate dynamically allocated arrays. 
    The so-called equivalence between arrays and pointers is so close, however,
    that programmers sometimes lose sight of the remaining essential differences between the two,
    imagining either that they are identical or that various nonsensical similarities of identities can be assumed between them.

C의 강점은 배열과 pointer의 유사성이다. 포인터를 통해 배열에 편리하게 접근하고, 동적으로 할당된 어레이를 simulate 할 수 있다. 그러나 포인터와 배열의, 소위 'equivalence'함이 너무나 가까운 나머지 둘 사이에 분명한 차이가 있음에도 불구하고 **동등**하다고 착각한다.

    The cornerstone of the “equivalence” of arrays and pointers in C is the fact that most array references decay into pointers to the array's first element as described in question [*]6.3. 
    Therefore, arrays are “second-class citizens” in C: You can never manipulate an array in its entirely (i.e., to copy it or pass it to a function),
    because whenever you mention its name, you're left with a pointer rather than the entire array.
    Because arrays deacy to pointers, the array subscripting operator [] always find itself, deep down, operating on a pointer.
    In fact, the subscripting expression a[i] is defined in terms of the equivalent pointer expression *((a) + (i)).

C에서 배열과 포인터가 "equivalence"한 것은, 대부분의 배열 reference가 배열의 첫번째 요소를 가리키는 포인터로 '퇴화'되는것에 기초한다. 따라서, C에서 배열은 **2등 시민**이다.
배열 이름 그 자체를 사용할 때 배열 전체가 사용되는것이 아닌 포인터가 남게 된다. 따라서, 당신은 배열 그 자체만을 복사하거나 함수에 전달하는 식의 사용은 불가능하다. 배열이 포인터로 퇴화(decay)되기에, []연산자는  항상 포인터에서 동작한다. 실제로 첨자표현식 a[i]는 포인터표현식*((a) + (i))로 정의된다. 

위 내용은 [pointer_1](./pointer_1.md) 문서에서 간단하게 다루었다. 다시 한 번 확인해보자.

![](/img/function_13.PNG)

이제서야 위의 내용이 제대로 이해가 가는 기분이다.

# char  a[]와 char *a

char a[]와 char *a 는 서로 연관되어있고, 비슷하게 쓰인다. 하지만, 둘은 전혀 다르다.

char a[6]와 같은 선언은 문자 6개를 저장할 수 있는 공간을 요청하고, 그 결과 a라는 이름이 이 공간을 대표한다.

반면에 char *p는 포인터를 저장할 수 있는 공간을 요구하고, p라는 이름이 이 공간을 대표한다. 이 포인터는 이제 어느것이든 가리킬 수 있다. 그림으로 확인해 보자.

    char a[] = "hello";
    char *p = "world";

위의 두 선언은 다음과 같이 데이터를 초기화한다.

![](/img/function.gif)


자, 이제 둘을 직접 비교해보자.

컴파일러가 a[3]을 봤을때, a라고 이름지어진 곳에서 3 만큼 떨어진 곳에 있는 데이터를 **불러온다.** 결과는 'l'이 될 것이다.

컴파일러가 p[3]을 보면, p라고 이름지어진 곳에서 3만큼 더한 곳에 있는 데이터를 **가리킨다.** 결과는 또한 'l'이 될 것이다.

둘 다 결과적으로 'l'을 가리키지만, 컴파일러가 사용하는 코드는 **다르다**는 사실을 기억하자.

# arr 와 &arr의 차이

포인터에 대하여 공부할 때 이 의문에 대한 궁금증이 완벽히 해소되지 않았다. 다행히도 공식 FAQ엔 이 궁금증을 해소할 수 있는 답이 있었다. 선 한 줄요약 : 

    arr 와 &arr는 타입이 다르다.

arr는 특정한 타입 T 형 배열 첫 번째 요소를 가리키는 포인터를 만든다 : &arr[0]

즉, T를 가리키는 포인터 (pointer to T)이다.

&arr는 배열 전체를 가리키는 포인터를 만든다. 즉, 이 포인터의 타입은 배열 T 전체를 가리키는 포인터 (pointer to array of T)이다. (오래된 C언어에선 &가 경고를 발생시킨다.)

간단한 예를 보자.

    int a[10];

이 배열에서 a에 대한 reference는 pointer to int라는 타입을 가진다.
반면에 &a에 대한 reference는 pointer to array of 10 ints 라는 타입이다.

이번엔 이차원 배열이다.

    int array[3][2]

위에서 차수만 하나 증가했다. 어려울 것 하나도 없다.

array의 타입은 pointer to array of 2 ints

&array의 타입은 pointer to array of 3 array of 2 ints 이다.

한번 직접 코디을 해보자.

    int main() {
        
        int arr[3];
        int* parr = arr;
        int(*parr2)[3] = &arr;

        printf("%p, %p\n", parr, parr+1);
        printf("%p, %p", parr2, parr2 + 1);

        return 0;
    }

![](/img/function_14.PNG)

그냥 arr을 대입한 parr은 +1을 하면 4가 증가한다. 반면에 &arr을 대입한 parr2는 +1을 하면 배열의 크기만큼인 12가 증가한다.

**궁금증 해결 완료**