## System.ComponentModel.Win32Exception

### Environment
- Language: C#
- IDE: Visual Studio

<br/>



### Problem
RequestNavigate 속성을 이용해서 특정 Uri로 이동하고자 할 때,
지정된 파일을 찾을 수 없다는 오류 발생

```html
<Hyperlink
    Name="song_link"
    TextDecorations="None"
    MouseEnter="song_link_MouseEnter"
    MouseLeave="song_link_MouseLeave"
    RequestNavigate="song_link_RequestNavigate"
    NavigateUri="https://www.naver.com">
    둥근해
</Hyperlink>
```

```cs
private void song_link_RequestNavigate(object sender, System.Windows.Navigation.RequestNavigateEventArgs e)
{
    System.Diagnostics.Process.Start(e.Uri.AbsoluteUri);
}
```

```bash
System.ComponentModel.Win32Exception: 'An error occurred trying to start process 'https://www.naver.com/' with working directory 'C:\Users\user\source\repos\Wpf_Basic_2\bin\Debug\net8.0-windows'. 지정된 파일을 찾을 수 없습니다.'
```

<br/>



### Cause of Problem
Hyperlink의 NavigateUri 속성은 일반적으로 파일 경로나 URL을 지정할 수 있음  
만약 NavigateUri 속성에 유효한 파일 경로가 주어진다면, 해당 파일이 실행되거나 열릴 수 있음  
그러나 만약 NavigateUri 속성에 URL이 주어진다면, 기본적으로 이 URL은 `파일로 인식`되어 특정 프로세스가 실행  

따라서, 'NavigateUri' 주소가 웹사이트로 연결되는 것이 아니라 파일로 인식되어 실행되는데, 이 때 해당 파일이 없어 오류 발생

<br/>



### Solution
WPF 애플리케이션에서 URL을 열기 위해서 UseShellExecute 속성을 true로 설정  
-> 기본 웹 브라우저를 사용하여 URL 연결  
`UseShellExecute = true` 추가

```cs
private void song_link_RequestNavigate(object sender, System.Windows.Navigation.RequestNavigateEventArgs e)
{
    Process.Start(new ProcessStartInfo(e.Uri.AbsoluteUri) { UseShellExecute = true });
    // System.Diagnostics.Process.Start(e.Uri.AbsoluteUri);
}
```

<br/>



### 📚 참고 자료
[How do you get NavigateUri to work in a WPF window?](https://stackoverflow.com/questions/21881124/how-do-you-get-navigateuri-to-work-in-a-wpf-window)  
