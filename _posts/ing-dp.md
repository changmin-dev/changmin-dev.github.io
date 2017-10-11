# 알고리즘
예전 면접이랑 알고리즘 문제에서 탈탈 털린 DP를 조금씩 해보려고 한다.

## LCS(최장 공통 부분 순열)
두 문자열(x,y)에서 공통적으로 나타나는 왼쪽에서 오른쪽으로 가는 가장 긴 글자들의 부분 순열을 찾는 문제이다.(연결이 되지 않아도 가능)
x = "ABCBDAB", y = "BDCABA" 라면 정답은 {"BCBA", "BDAB", "BCAB"}이다.

### 브루트 포스
길이 1인 경우 A,B,C,D
길이 2인 경우 AB, BA BB, BC,,BD, CA, CB, DA, DB, ...
길이 3인 경우 ABA, ....
길이 4인 경우 BCBA, BDAB, BCAB
이후 없음

무식하게 푸는 방법 - 모든 부분 순열을 검사 하는 방법(순열 부분은 추상화)
```
public static String[] LCS(String x, String y){
    String[] x_perm = permutationAllLength(x);
    String[] y_perm = permutationAllLength(y);

    for(int i=0, j=0; i < xch.length;){
        if(xch[i] 
    }
}
```
### 재귀
```
A B  C  BD AB
  BD CA B  A
```
두개의 첫글자가 다르면 공통 부분 수열의 일부가 되는것이 불가능하다.
### DP