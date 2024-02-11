## System.StackOverflowException

### Environment
- Language: C#
- IDE: Visual Studio

<br/>



### Problem
ListView의 SelectedItem을 SelectedCity을 설정했을 때, StackOverflowException 발생

```html
<ListView
    ItemsSource="{Binding Cities}"
    SelectedValue="{Binding SelectedCity}"
    >
```

```bash
System.StackOverflowException: 'Exception_WasThrown'
```

<br/>



### Cause of Problem
SelectedCity의 Set 속성에 SelectedCity의 대상이 되는 Cities의 항목을 지우는 메서드로 인해 SelectedCity가 null값으로 바뀌고 다시 Set 속성이 동작하면서 StackOverflowException 발생

```cs
private City _selectedCity;

public City SelectedCity
{
    get { return _selectedCity; }
    set 
    {
        _selectedCity = value;
        OnPropertyChanged("SelectedCity");
        GetCurrentConditions();
    }
}
```

```cs
private async void GetCurrentConditions()
{
    Query = string.Empty;
    Cities.Clear();

    CurrentConditions = await AccuWeatherHelper.GetCurrentConditions(SelectedCity.Key);
}
```

- Cities가 Clear되면서 SelectedCity가 null로 변경

<br/>



### Solution
SelectedItem 대신 SelectedValue로 변경하여 속성값에 직접 접근  
Cities가 null이 되더라도 SelectedCity의 값이 바로 변경되지 않기 때문에 OnPropertyChanged와 GetCurrentConditions()가 무한히 호출되는 상황을 방지할 수 있음

```html
<ListView
    ItemsSource="{Binding Cities}"
    SelectedValue="{Binding SelectedCity}"
    >
```

<br/>

### SelectedItem와 SelectedValue
- SelectedItem
    - 리스트에서 선택된 항목의 전체 객체를 참조
    - 아이템의 선택 상태가 변경될 때마다 즉시 바인딩된 속성을 업데이트
- SelectedValue
    - 선택된 항목의 특정 속성 값에 직접적으로 접근
    - SelectedValuePath 속성을 통해 설정
    - 선택 상태의 변경을 좀 더 지연시키고, 바인딩된 속성의 업데이트가 덜 민감하게 반응

```html
<!-- Example SelectedValue -->
<ListView
    ItemsSource="{Binding Cities}"
    SelectedValuePath="Key"
    SelectedValue="{Binding KeyVariable}"
    >
```

- ItemsSource: ListView Source
- SelectedValuePath: ItemsSource에서 가져올 값(Object의 필드)
- SelectedValue: ViewModel에 Binding할 변수

<br/>



### 📚 참고 자료

[[C# wpf] 콤보박스 selecteditem vs selectedvalue 차이점](https://yeko90.tistory.com/entry/wpf-%EC%BD%A4%EB%B3%B4%EB%B0%95%EC%8A%A4-selecteditem-selectedvalue-%EC%B0%A8%EC%9D%B4)  