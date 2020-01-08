## 1. WinForm, WPF
Thread
1. UI Thread 
2. Background Thread
    - for, foreach, 파일 입출력
  
## 2. 동기적 프로그래밍
 Method1(); 3초
 <br>Method2(); 3초
 <br>Method3(); 4초
 <br> - 총 10초 소요
  
## 3. 비동기적 프로그래밍
<br> Method1(); 3초
<br> Method2(); 3초
<br> Method3(); 4초
<br> - 총 4초 소요

## 4. C# 비동기적 프로그램을 위한 키워드
async, await,  Task, Task<T>
 - C# 5.0, 닷넷프레임워크 4.5 이상 지원
 
## 5. 비동기으로 처리하는 것이 과연 성능상 좋아진 것일까?
  - 아니다..단지 남는 컴퓨터 자원을 이용해서 응답시간이 단축될 뿐 

## 6. source 코드
- 진단도구 이용한 방식 권장
- 비동기 문 사용시 주의!!! (var result1 = await Test1Async() => 비동기 처리 안됨!)

 ```C#
async 메소드 이름 convention => Async : MS 권장 사항 
return type은 <> 안에
public async Task<int> Test1Async() 
{
    await Task.Delay(3000);
    return 0;
} 
public async Task<int> Test2Async() 
{
    await Task.Delay(3000);
    return 0;
}
public async Task<int> Test3Async() 
{
    await Task.Delay(3000);
    return 0;
} 

public async Task<IActionResult> Contact()
{
    // # 테스트 방법 - 전통적 방식
    //Stopwatch watch = new Stopwatch();
    //watch.Start();
    
    // # 새로운 테스트 방식 - VS Diagonostics Tools (진단도구) : Debug > Windows 에 있음
    //시작하는 위치, 종료 후 위치 Break Point    
    
    //await Test1Async();  //3초 기다림, 메모리 상에 올려서  await를 시키지 않음
    var test1 =  Test1Async();  
    var test2 =  Test2Async();
    var test3 =  Test3Async();
    
    var result1 = await test1;  //반환값
    var result2 = await test2;  //반환값
    var result3 = await test3;  //반환값
    
    //이렇게 해도 비동기적 처리가 안됨 (조심!)
    //var result1 = await Test1Async();
    
    // # 테스트 방법 - 전통적 방식
    //watch.Stop();
    //var result = watch.ElapsedMilliseconds;
    
    return View();
}
```
