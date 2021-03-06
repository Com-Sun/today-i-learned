 ## 문제

 9개의 서로 다른 자연수가 주어질 때, 이들 중 최댓값을 찾고 그 최댓값이 몇 번째 수인지를 구하는 프로그램을 작성하시오.

예를 들어, 서로 다른 9개의 자연수

3, 29, 38, 12, 57, 74, 40, 85, 61

이 주어지면, 이들 중 최댓값은 85이고, 이 값은 8번째 수이다.

 ## 입력

첫째 줄부터 아홉 번째 줄까지 한 줄에 하나의 자연수가 주어진다. 주어지는 자연수는 100 보다 작다.

 ## 출력

첫째 줄에 최댓값을 출력하고, 둘째 줄에 최댓값이 몇 번째 수인지를 출력한다.
 ## 코드
 
 
    #define _CRT_SECURE_NO_WARNINGS
    #include <stdio.h>


    int main() {

        int arr[9];
        int input;
        int count = 0;
        int max;

        for (int i = 0; i < 9; i++) {
            scanf("%d", &input);
            arr[i] = input;
        }

        max = arr[0];
        for (int i = 0; i < 9; i++) {

            if (max < arr [i]) {
                max = arr[i];
            }
        }

        for (int i = 0; i < 9; i++) {
            if (max == arr[i]) {
                count = i;
            }
        }
        printf("%d\n%d", max, count+1);

        return 0;
    }

![](/img/max.PNG)

## 풀이

크기가 9인 배열을 만든 후, max 변수에 배열의 첫 번째 변수를 할당했다.
배열의 각 요소는 반복문을 통해 할당했다.

헷갈렸던 부분은 최대값이 몇 번째에 위치하는지를 구하는 방법이었다.
처음에 썼던 코드는 max와 배열을 비교해서 count ++ 를 하는 방법이었다.
하지만 숫자가 랜덤하게 나올 경우 제대로 작동하지 않는다는 오류가 있었다.
고민 끝에 max 와 i번째 배열이 같다는 것에 착안, max == arr[i] 라는 코드를 써서 문제를 풀었다.

### TAG : 구현
