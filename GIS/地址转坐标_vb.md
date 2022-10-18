# 地址转坐标_vb
```vb
Attribute VB_Name = "地址转经纬度"
Public Sub 地址转经纬度()
    
    '创建对象
    Dim oDom As Object
    Dim objsc As Object
    Dim iRows As Integer
    Dim address1 As String
    
    iRows = ActiveSheet.UsedRange.Rows.Count
    Set objsc = CreateObjectx86("MSScriptControl.ScriptControl")   '在64位版Excel中的处理方法
    objsc.Language = "JScript"
    For i = 2 To iRows
        address1 = Cells(i, "A").Value
        Address = UrlEncode(address1)
        URL = "https://restapi.map.so.com/newapi?d=pc&keyword=" + Address + "&cityname=%E6%B9%96%E5%8C%97%E7%9C%81&sid=1000"
        Dim http As Object
        Set http = CreateObject("Microsoft.XMLHTTP")     ' 创建 http 对象以发送请求
        http.Open "GET", URL, False                      ' 设置请求地址
        http.setRequestHeader "CONTENT-TYPE", "application/x-www-form-urlencoded"     '设置请求头
        http.send    '发送请求
        If http.Status = 200 Then
            Dim json$                      '定义字符串 json
            json = http.responseText       '获取相应结果
            '接下来是解析 json
            strJSON = "var json=" & json
            objsc.AddCode (strJSON)        '将 json 由字符串解析为对象
            Dim geocodes
            geocodes = objsc.Eval("json.geocodes")
            Sheets(1).Cells(i, 3).Value = objsc.Eval("json.poi[0].x") '经度
            Sheets(1).Cells(i, 4).Value = objsc.Eval("json.poi[0].y") '纬度
        Else
        Sheets(1).Cells(i, 5) = "未查询到"
        End If
    Next
End Sub
Function CreateObjectx86(Optional sProgID, Optional bClose = False)
    Static oWnd As Object
    Dim bRunning As Boolean
    #If Win64 Then
        bRunning = InStr(TypeName(oWnd), "HTMLWindow") > 0
        If bClose Then
            If bRunning Then oWnd.Close
            Exit Function
        End If
        If Not bRunning Then
            Set oWnd = CreateWindow()
            oWnd.execScript "Function CreateObjectx86(sProgID): Set CreateObjectx86 = CreateObject(sProgID): End Function", "VBScript"
        End If
        Set CreateObjectx86 = oWnd.CreateObjectx86(sProgID)
    #Else
        Set CreateObjectx86 = CreateObject("MSScriptControl.ScriptControl")
    #End If
End Function
Function CreateWindow()
    Dim sSignature, oShellWnd, oProc
    On Error Resume Next
    sSignature = Left(CreateObject("Scriptlet.TypeLib").GUID, 38)
    CreateObject("WScript.Shell").Run "%systemroot%\syswow64\mshta.exe about:""about:<head><script>moveTo(-32000,-32000);document.title='x86Host'</script><hta:application showintaskbar=no /><object id='shell' classid='clsid:8856F961-340A-11D0-A96B-00C04FD705A2'><param name=RegisterAsBrowser value=1></object><script>shell.putproperty('" & sSignature & "',document.parentWindow);</script></head>""", 0, False
    Do
        For Each oShellWnd In CreateObject("Shell.Application").Windows
            Set CreateWindow = oShellWnd.GetProperty(sSignature)
            If Err.Number = 0 Then Exit Function
            Err.Clear
        Next
    Loop
End Function
Function UrlEncode(ByRef szString As String) As String
    Dim szChar   As String
    Dim szTemp   As String
    Dim szCode   As String
    Dim szHex    As String
    Dim szBin    As String
    Dim iCount1  As Integer
    Dim iCount2  As Integer
    Dim iStrLen1 As Integer
    Dim iStrLen2 As Integer
    Dim lResult  As Long
    Dim lAscVal  As Long
    szString = Trim$(szString)
    iStrLen1 = Len(szString)
    For iCount1 = 1 To iStrLen1
        szChar = Mid$(szString, iCount1, 1)
        lAscVal = AscW(szChar)
        If lAscVal >= &H0 And lAscVal <= &HFF Then
            If (lAscVal >= &H30 And lAscVal <= &H39) Or _
                (lAscVal >= &H41 And lAscVal <= &H5A) Or _
                (lAscVal >= &H61 And lAscVal <= &H7A) Then
            szCode = szCode & szChar
        Else
            szCode = szCode & "%" & Hex(AscW(szChar))
        End If
    Else
        szHex = Hex(AscW(szChar))
        iStrLen2 = Len(szHex)
        For iCount2 = 1 To iStrLen2
            szChar = Mid$(szHex, iCount2, 1)
            Select Case szChar
            Case Is = "0"
                szBin = szBin & "0000"
            Case Is = "1"
                szBin = szBin & "0001"
            Case Is = "2"
                szBin = szBin & "0010"
            Case Is = "3"
                szBin = szBin & "0011"
            Case Is = "4"
                szBin = szBin & "0100"
            Case Is = "5"
                szBin = szBin & "0101"
            Case Is = "6"
                szBin = szBin & "0110"
            Case Is = "7"
                szBin = szBin & "0111"
            Case Is = "8"
                szBin = szBin & "1000"
            Case Is = "9"
                szBin = szBin & "1001"
            Case Is = "A"
                szBin = szBin & "1010"
            Case Is = "B"
                szBin = szBin & "1011"
            Case Is = "C"
                szBin = szBin & "1100"
            Case Is = "D"
                szBin = szBin & "1101"
            Case Is = "E"
                szBin = szBin & "1110"
            Case Is = "F"
                szBin = szBin & "1111"
            Case Else
            End Select
        Next iCount2
        szTemp = "1110" & Left$(szBin, 4) & "10" & Mid$(szBin, 5, 6) & "10" & Right$(szBin, 6)
        For iCount2 = 1 To 24
            If Mid$(szTemp, iCount2, 1) = "1" Then
                lResult = lResult + 1 * 2 ^ (24 - iCount2)
                Else: lResult = lResult + 0 * 2 ^ (24 - iCount2)
            End If
        Next iCount2
        szTemp = Hex(lResult)
        szCode = szCode & "%" & Left$(szTemp, 2) & "%" & Mid$(szTemp, 3, 2) & "%" & Right$(szTemp, 2)
        End If
        szBin = vbNullString
        lResult = 0
    Next iCount1
    UrlEncode = szCode
End Function
```