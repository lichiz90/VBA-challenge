# VBA-challenge[Week 2 Challenge.xlsx](https://github.com/user-attachments/files/16631326/Week.2.Challenge.xlsx)
![image](https://github.com/user-attachments/assets/4b49a072-606a-4e7d-8bc2-8f8afe465aa2)
![image](https://github.com/user-attachments/assets/ca456a8a-bb31-48aa-ae0c-e7993d7f9f1f)
![image](https://github.com/user-attachments/assets/70bbf3b9-2bda-4d1a-bedc-30b53e243e9a)
![image](https://github.com/user-attachments/assets/bd9f88e1-22b2-47d7-8fb8-9e8a40fcf7ac)
![image](https://github.com/user-attachments/assets/9a5172a2-a1f6-48a6-86cd-d42b88b93b0b)
Code Used: Sub WorksheetLoop()

    Dim ws As Worksheet

    ' Begin the loop.
    For Each ws In Worksheets
        
        'Ticker & Dates
            'Declare Variables
            Dim NumRow As Variant
            Dim SubNumRow As Variant
            Dim DeDup As Variant
            Dim yrOpen As String
            Dim yrClose As String
            
            'Copy Tickers to New Column
            NumRow = ws.Range("A2").End(xlDown).Row
            ws.Range("A2:A" & NumRow).Copy ws.Range("I2:I" & NumRow)
            
            'DeDup the new ticker column
            Set DeDup = ws.Range("I1:I" & NumRow)
            DeDup.RemoveDuplicates Columns:=1, Header:=xlYes
            SubNumRow = ws.Range("I2").End(xlDown).Row
    
            'Set opening and closing dates
            yrOpen = Left(ws.Cells(2, 2).Value2, 4) & "0102"
            yrClose = Left(ws.Cells(2, 2).Value2, 4) & "1231"

        'Calculations per ticker

            'Declare Variables
            Dim valOpen As Variant
            Dim valClose As Variant
            Dim Ticker As Variant
            Dim Yearly As Variant
            Dim PercentChange As Variant
            Dim Volume As Variant
            Dim maxUpTicker As Variant
            Dim maxUpVal As Variant
            Dim maxDownTicker As Variant
            Dim maxDownVal As Variant
            Dim maxVolTicker As Variant
            Dim maxVolVal As Variant
            Dim NewTickerRow As Variant
            
            NewTickerRow = 2

            For c = 2 To SubNumRow
                Ticker = ws.Cells(c, 9).Value2
                Volume = 0
                
                For d = NewTickerRow To NumRow
                    'Find opening and closing Values
                    If ws.Cells(d, 2).Value2 = yrOpen And ws.Cells(d, 1).Value2 = Ticker Then
                       valOpen = ws.Cells(d, 3).Value2
                    ElseIf ws.Cells(d, 2).Value2 = yrClose And ws.Cells(d, 1).Value2 = Ticker Then
                       valClose = ws.Cells(d, 6).Value2
                    End If

                    'Find Total Volume
                    If ws.Cells(d, 1).Value2 = Ticker Then
                        Volume = Volume + ws.Cells(d, 7).Value2
                    Else
                        NewTickerRow = d
                        Exit For
                    End If
                Next d

                Yearly = valClose - valOpen
                PercentChange = (valClose - valOpen) / valOpen

                ws.Cells(c, 11).Value2 = PercentChange
                ws.Cells(c, 11).NumberFormat = "0.00%"
                ws.Cells(c, 10).Value2 = Yearly

                If ws.Cells(c, 10).Value2 < 0 Then
                    ws.Cells(c, 10).Interior.Color = vbRed
                ElseIf ws.Cells(c, 10).Value2 > 0 Then
                    ws.Cells(c, 10).Interior.Color = vbGreen
                End If

                ws.Cells(c, 12).Value2 = Volume
                
                
                'Find Overall Calculations
                If ws.Cells(c, 11).Value2 > maxUpVal Then
                    maxUpVal = ws.Cells(c, 11).Value2
                    maxUpTicker = ws.Cells(c, 9).Value2
                End If

                If ws.Cells(c, 11).Value2 < maxDownVal Then
                    maxDownVal = ws.Cells(c, 11).Value2
                    maxDownTicker = ws.Cells(c, 9).Value2
                End If

                If ws.Cells(c, 12).Value2 > maxVolVal Then
                    maxVolVal = ws.Cells(c, 12).Value2
                    maxVolTicker = ws.Cells(c, 9).Value2
                End If
                
            Next c

            ws.Cells(1, 9).Value2 = "Ticker"
            ws.Cells(1, 10).Value2 = "Yearly Change"
            ws.Cells(1, 11).Value2 = "Percent Change"
            ws.Cells(1, 12).Value2 = "Total Stock Volume"
            ws.Cells(1, 17).Value2 = "Ticker"
            ws.Cells(1, 18).Value2 = "Value"
            ws.Cells(2, 16).Value2 = "Greatest % Increase"
            ws.Cells(3, 16).Value2 = "Greatest % Decrease"
            ws.Cells(4, 16).Value2 = "Greatest Total Volume"



            ws.Cells(2, 17).Value2 = maxUpTicker
            ws.Cells(3, 17).Value2 = maxDownTicker
            ws.Cells(4, 17).Value2 = maxVolTicker


            ws.Cells(2, 18).Value2 = maxUpVal
            ws.Cells(3, 18).Value2 = maxDownVal
            ws.Cells(4, 18).Value2 = maxVolVal

            ws.Cells(2, 18).NumberFormat = "0.00%"
            ws.Cells(3, 18).NumberFormat = "0.00%"
            ws.Cells(4, 18).NumberFormat = "0"

        Next
        
           
End Sub
    

