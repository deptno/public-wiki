Welcome to the [[deptno.dev]]!

## 보일러 배관
```mermaid 
flowchart TD
  분배 --label:침실3--> 1
  분배 --label:침실2--> 2
  분배 --label:침실1--> 3
  분배 --label:침실1--> 4
  분배 --label:거실2--> 5
  분배 <-. 온수 .- 6
  보일러 -- 온수 --> 6
  1 --> 침실
  2 --> 오락실
  3 --> 작업실
  4 -- "✅ 창가 오른쪽" --> 작업실
  5 --> 거실
```

## link
- [[smarthome]]
