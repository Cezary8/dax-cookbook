# Miscellaneous
Snippets that are difficult to categorize

## Filtering out empty
  Drop this measure into slicer's visual level filters and filter to 1
  That way you will avoid both type realtionship
  ```
  filter empty items = 
    if(
        COUNTROWS('f logs')>0
        ,1,0
    )
  ```

## Diplaying applied filters
  This measure intercept applied filters and display them (both fields and criteria)
  ```
  Show Filters = 
  var _max = 2
  return
  ------YEARS-------------
  "Year = " & 'param year'[selected year] & UNICHAR(13) & UNICHAR(10) &
  ------MONTHS-------------
  IF( 
      ISFILTERED('d calendar'[month]),
      var _f = FILTERS('d calendar'[month])
      var _r = COUNTROWS(_f)
      var _t = TOPN(_max, _f, 'd calendar'[month])
      var _d = CONCATENATEX(_t, 'd calendar'[month], ", ")
      var _x = "Month = " & _d & IF(_r > _max, ", ... [" & _r & " items selected]") & " "
      return _x & UNICHAR(13) & UNICHAR(10)
  )------DAY-------------
  & IF( 
      ISFILTERED('d calendar'[day]),
      var _f = FILTERS('d calendar'[day])
      var _r = COUNTROWS(_f)
      var _t = TOPN(_max, _f, 'd calendar'[day])
      var _d = CONCATENATEX(_t, 'd calendar'[day], ", ")
      var _x = "Day = " & _d & IF(_r > _max, ", ... [" & _r & " items selected]") & " "
      return _x & UNICHAR(13) & UNICHAR(10)
  )------DOMAIN-------------
  &
  IF( 
      ISFILTERED('f logs'[domain]),
      var _f = FILTERS('f logs'[domain])
      var _r = COUNTROWS(_f)
      var _t = TOPN(_max, _f, 'f logs'[domain])
      var _d = CONCATENATEX(_t, 'f logs'[domain], ", ")
      var _x = "Domain = " & _d & IF(_r > _max, ", ... [" & _r & " items selected]") & " "
      return _x & UNICHAR(13) & UNICHAR(10)
  )------SM users group-------------
  &
  IF( 
      ISFILTERED('d ad_users'[sm user group]),
      var _f = FILTERS('d ad_users'[sm user group])
      var _r = COUNTROWS(_f)
      var _t = TOPN(_max, _f, 'd ad_users'[sm user group])
      var _d = CONCATENATEX(_t, 'd ad_users'[sm user group], ", ")
      var _x = "SM users group = " & _d & IF(_r > _max, ", ... [" & _r & " items selected]") & " "
      return _x & UNICHAR(13) & UNICHAR(10)
  )------STATUS - DISPLAY NAME-------------
  & IF( 
      ISFILTERED('d ad_users'[display_name]),
      var _f = FILTERS('d ad_users'[display_name])
      var _r = CALCULATE(DISTINCTCOUNT('d ad_users'[display_name]),'f logs')--COUNTROWS(_f)
      var _t = TOPN(_max, _f, 'd ad_users'[display_name])
      var _d = CONCATENATEX(_t, 'd ad_users'[display_name], ", ")
      var _x = "Name = " & _d & IF(_r > _max, ", ... [" & _r & " items selected]") & " "
      return _x & UNICHAR(13) & UNICHAR(10),
          IF( 
              ISFILTERED('d grades'[caption]),
              var _f = FILTERS('d grades'[caption])
              var _r = COUNTROWS(_f)
              var _t = TOPN(_max, _f, 'd grades'[caption])
              var _d = CONCATENATEX(_t, 'd grades'[caption], ", ")
              var _x = "Status = " & _d & IF(_r > _max, ", ... [" & _r & " items selected]") & " "
              return _x & UNICHAR(13) & UNICHAR(10)
          )
  )------COUNTRY - CITY-------------
  & IF( 
      ISFILTERED('d ad_users'[city]),
      var _f = FILTERS('d ad_users'[city])
      var _r = CALCULATE(DISTINCTCOUNT('d ad_users'[city]),'f logs')--COUNTROWS(_f)
      var _t = TOPN(_max, _f, 'd ad_users'[city])
      var _d = CONCATENATEX(_t, 'd ad_users'[city], ", ")
      var _x = "City = " & _d & IF(_r > _max, ", ... [" & _r & " items selected]") & " "
      return _x & UNICHAR(13) & UNICHAR(10),
          IF( 
              ISFILTERED('d ad_users'[country]),
              var _f = FILTERS('d ad_users'[country])
              var _r = COUNTROWS(_f)
              var _t = TOPN(_max, _f, 'd ad_users'[country])
              var _d = CONCATENATEX(_t, 'd ad_users'[country], ", ")
              var _x = "Country = " & _d & IF(_r > _max, ", ... [" & _r & " items selected]") & " "
              return _x & UNICHAR(13) & UNICHAR(10)
          )
  )------DEPARTMENT-------------
  &
  IF( 
      ISFILTERED('d ad_users'[department]),
      var _f = FILTERS('d ad_users'[department])
      var _r = COUNTROWS(_f)
      var _t = TOPN(_max, _f, 'd ad_users'[department])
      var _d = CONCATENATEX(_t, 'd ad_users'[department], ", ")
      var _x = "Department = " & _d & IF(_r > _max, ", ... [" & _r & " items selected]") & " "
      return _x & UNICHAR(13) & UNICHAR(10)
  )------DIVISION-------------
  &
  IF( 
      ISFILTERED('d ad_users'[division]),
      var _f = FILTERS('d ad_users'[division])
      var _r = COUNTROWS(_f)
      var _t = TOPN(_max, _f, 'd ad_users'[division])
      var _d = CONCATENATEX(_t, 'd ad_users'[division], ", ")
      var _x = "Division = " & _d & IF(_r > _max, ", ... [" & _r & " items selected]") & " "
      return _x & UNICHAR(13) & UNICHAR(10)
  )------OFFICE-------------
  &
  IF( 
      ISFILTERED('d ad_users'[office_location]),
      var _f = FILTERS('d ad_users'[office_location])
      var _r = COUNTROWS(_f)
      var _t = TOPN(_max, _f, 'd ad_users'[office_location])
      var _d = CONCATENATEX(_t, 'd ad_users'[office_location], ", ")
      var _x = "Office = " & _d & IF(_r > _max, ", ... [" & _r & " items selected]") & " "
      return _x & UNICHAR(13) & UNICHAR(10)
  )
  ```

## Custom hyperlink icon
  Based on image transformed into base64 format
  Implement that as a calculated column and assign "Umage URL" Data category
  Choose in data formatting option
  
  ```
  "data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4KPCEtLSBHZW5lcmF0b3I6IEFkb2JlIElsbHVzdHJhdG9yIDI3LjguMCwgU1ZHIEV4cG9ydCBQbHVnLUluIC4gU1ZHIFZlcnNpb246IDYuMDAgQnVpbGQgMCkgIC0tPgo8c3ZnIHZlcnNpb249IjEuMSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB4bWxuczp4bGluaz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayIgeD0iMHB4IiB5PSIwcHgiCgkgdmlld0JveD0iMCAwIDIwMCAyMDAiIHN0eWxlPSJlbmFibGUtYmFja2dyb3VuZDpuZXcgMCAwIDIwMCAyMDA7IiB4bWw6c3BhY2U9InByZXNlcnZlIj4KPHN0eWxlIHR5cGU9InRleHQvY3NzIj4KCS5zdDB7ZmlsbDojRkZGRkZGO3N0cm9rZTojMDAwMDAwO3N0cm9rZS13aWR0aDowLjI4MzU7c3Ryb2tlLW1pdGVybGltaXQ6MTA7fQoJLnN0MXtmaWxsOm5vbmU7c3Ryb2tlOiMwMDAwMDA7c3Ryb2tlLXdpZHRoOjAuMjgzNTtzdHJva2UtbWl0ZXJsaW1pdDoxMDt9Cgkuc3Qye2ZpbGw6IzQ3NDAzRDt9Cgkuc3Qze2ZpbGw6I0IzQjJCNDt9Cgkuc3Q0e2ZpbGw6bm9uZTt9Cgkuc3Q1e2ZpbGw6bm9uZTtzdHJva2U6IzAwMDAwMDtzdHJva2Utd2lkdGg6ODtzdHJva2UtbWl0ZXJsaW1pdDoxMDt9Cgkuc3Q2e2ZpbGw6I0ZGRkZGRjtzdHJva2U6IzAwMDAwMDtzdHJva2Utd2lkdGg6ODtzdHJva2UtbWl0ZXJsaW1pdDoxMDt9Cgkuc3Q3e2ZpbGw6I0ZGRkZGRjtzdHJva2U6IzAwMDAwMDtzdHJva2Utd2lkdGg6OC41MDM5O3N0cm9rZS1taXRlcmxpbWl0OjEwO30KPC9zdHlsZT4KPGcgaWQ9IkdyaWQiPgo8L2c+CjxnIGlkPSJJY29ucyI+Cgk8Zz4KCQk8Zz4KCQkJPHJlY3QgeD0iOTYiIHk9IjgzIiBjbGFzcz0ic3QyIiB3aWR0aD0iOCIgaGVpZ2h0PSI1MS45Ii8+CgkJPC9nPgoJCTxnPgoJCQk8cmVjdCB4PSI5NiIgeT0iNjciIGNsYXNzPSJzdDIiIHdpZHRoPSI4IiBoZWlnaHQ9IjgiLz4KCQk8L2c+CgkJPGc+CgkJCTxwYXRoIGNsYXNzPSJzdDIiIGQ9Ik0xMDAsMTcwLjVjLTM4LjgsMC03MC40LTMxLjYtNzAuNC03MC40YzAtMzguOCwzMS42LTcwLjUsNzAuNC03MC41YzM4LjgsMCw3MC40LDMxLjYsNzAuNCw3MC41CgkJCQlDMTcwLjQsMTM4LjksMTM4LjgsMTcwLjUsMTAwLDE3MC41eiBNMTAwLDM3LjZjLTM0LjQsMC02Mi40LDI4LTYyLjQsNjIuNWMwLDM0LjQsMjgsNjIuNCw2Mi40LDYyLjRjMzQuNCwwLDYyLjQtMjgsNjIuNC02Mi40CgkJCQlDMTYyLjQsNjUuNiwxMzQuNCwzNy42LDEwMCwzNy42eiIvPgoJCTwvZz4KCTwvZz4KPC9nPgo8L3N2Zz4K"
  ```
