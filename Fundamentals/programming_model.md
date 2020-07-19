### 1.1.20
#### (1) 证明: ln(m*n) = ln(m) + ln(n)
    m * n = e^ln(m) * e^ln(n)
                = e^(ln(m) + ln(n))
    故：ln(m*n) = ln(m) + ln(n)
#### (2) 实现如下：
```java
    public static double logN(int N){
        if(N == 0 || N == 1)
            return N;
        
        return Math.log(N) + logN(N - 1);
    }
```

### 1.1.21
```java
        Pattern pattern = Pattern.compile("^(.+?)\\s+(\\d+)\\s+(\\d+).*$");
        Scanner scanner = new Scanner(System.in);
        StringBuilder builder = new StringBuilder();
        while (true) {
            String line = scanner.nextLine();
            if (line == null || "".equals(line)) {
                break;
            }

            Matcher matcher = pattern.matcher(line);
            if (matcher.find()) {
                String name = matcher.group(1);
                int score = Integer.parseInt(matcher.group(2));
                int total = Integer.parseInt(matcher.group(3));
                if (total == 0)
                    continue;

                builder.append(name);
                builder.append(" ");
                builder.append(score);
                builder.append(" ");
                builder.append(total);
                builder.append(" ");
                builder.append(String.format("%.3f", 1.0 * score /  total));
                builder.append(System.lineSeparator());
            }
        }

        System.out.print(builder.toString());
```

### 1.1.22

```java
    public static int rank(int key, int[] a){
        return rank(key, a, 0, a.length - 1, 0);
    }

    public static int rank(int key, int[] a, int lo, int hi, int deepth){
        for(int i=0; i<deepth; i++){
            System.out.print(" ");
        }

        System.out.printf("lo=%d, hi=%d%s", lo, hi, System.lineSeparator());
        //如果key存在于a[]中，它的索引不会小于lo且不会大于hi
        if(lo > hi)
            return -1;

        int mid = lo + (hi - lo) / 2;
        if(key < a[mid])
            return rank(key, a, lo, mid - 1, deepth + 1);
        else if(key > a[mid])
            return rank(key, a, mid + 1, hi, deepth + 1);
        else
            return mid;
    }
```

### 1.1.24

```java
    public static int gdc(int a, int b){
        System.out.printf("p=%d, q=%d%s", a, b, System.lineSeparator());
        int c = a % b;
        if(c == 0)
            return b;

        return gdc(b, c);
    }
```