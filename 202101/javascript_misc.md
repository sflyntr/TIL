```
var searchForm = $(".search-form")
searchForm.find("[name='q']")
```

### jQuery SELECTOR
- jquery selector를 쓴것이다.

> Attribute Equals Selector 
> find("[name=”value”]) 형식이다.
>> 따라서 위의 것은 form안에 name='q'는 속성name이 q인 object를 select하는 것이다.
>> form안에 속성명이 q 인것에 대한 value는 searchForm.find("[name='q']").val()이다.
>> 이는 form data로 만약에 q='hat' 이런식으로 보내면 hat을 뽑아낸다.

* 참조링크 : <https://api.jquery.com/category/selectors/> 

### jQuery Time 관련

- 실행시켜주는 함수
setTimeout("실행시킬 함수",시간) : 대기 시간이 지난 후 해당 함수를 한번만 실행시킨다.
setInterval("실행시킬 함수",시간) : 대기 시간이 지난 후 해당 함수를 반복해서 실행시킨다.

- 작동을 중지시키는 함수
clearTimeout() : setTimeout을 중지 시킨다.
clearInterval() : setInterval을 중지 시킨다.

```javascript

```
