# Time intelligence measures

## Technical measures
  Used as a reference in time intelligence functions

  ```
  max year = 
  var last_date = 
      CALCULATE(
          LASTNONBLANK('f logs'[log_dt],[entries]),
          ALL()
      )
  return
  year(last_date) -- MONTH(last_date) or day(last_date) - depending whether you calculate max month or day
  formatString: #,0
  ```

## This year (TY)

  ```
  entries TY =
  CALCULATE(
      [entries],
      FILTER(
          'd calendar',
          'd calendar'[year] = [selected year]
      )
  )
  formatString: #,0
  ```

## Previous year (PY)

  ```
  entries PY =
  CALCULATE(
      [entries],
      FILTER(
          'd calendar',
          'd calendar'[year] = [selected year] -1 &&
          'd calendar'[date] <= DATE([max year] - 1,[max month],[max day])
      )
  )
  formatString: #,0
  ```

## Previous year (PY) full
  Used as a benchmark in line charts in which I want to see full year perspective, nto like for like

  ```
  entries PY full =
  CALCULATE(
      [entries],
      FILTER(
          'd calendar',
          'd calendar'[year] = [selected year] -1 
      )
  )
  formatString: #,0
  ```

## diff%

  ```
  entries vsPY% =
  DIVIDE(
      ([entries TY]-[entries PY]),
      [entries PY]
  )
  formatString: +0%;-0%;0%
  ```

## Most recent time/date

  ```
  Most recent time = 
  var _dt =
  CALCULATE(
      LASTDATE('f logs'[log_dt]),
      all()
  )
  var  _recent =
  CALCULATE(
      MAX('f logs'[log_tm]),
      FILTER(
          all('f logs'),
          'f logs'[log_dt] = _dt
      )
  )
  var _caption = "Most recent data: "
  RETURN
  _caption & FORMAT(_recent, "Short Time", "en-EN")
  --_caption & FORMAT(_dt, "Short Date", "en-EN") /////////for recent date
  ```

## Autocalendar
  Based on min and max date in fact table

  ```
  Date = CALENDAR(MIN(Utils_Years[Date]), DATE(YEAR(MAX(Utils_Years[Date])),12,31))
  ```
