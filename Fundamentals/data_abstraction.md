<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

# 习题1.2

## 提高题

### 1.2.15 文件输入。基于 String 的 split() 方法实现 In 中的静态方法 readInts()。

```java
public static int[] readInts(){
	String content = readAll();
	String[] params = content.split("\\s");
	int[] result = new int[params.length];
	for(int i=0; i<params.length; i++){
		result[i] = Integer.parseInt(params[i]);
	}

	return result;
}
```

### 1.2.16 有理数。为有理数实现一个不可变数据类型Rational，支持加减乘除操作。

<table>
	<tbody>
	<tr>
		<td>public class Rational</td>
	</tr>
	<tr>
		<td>Rational(int numerator, int denominator)</td>
	</tr>
	<tr>
		<td>Rational plus(Rational b)</td>
		<td>该数与b之和</td>
	</tr>
	<tr>
		<td>Rational minus(Rational b)</td>
		<td>该数与b之差</td>
	</tr>
	<tr>
		<td>Rational times(Rational b)</td>
		<td>该数与b之积</td>
	</tr>
	<tr>
		<td>Rational divides(Rational b)</td>
		<td>该数与b之商</td>
	</tr>
	<tr>
		<td>boolean equals(Rational that)</td>
		<td>该数与that相等吗</td>
	</tr>
	<tr>
		<td>String toString()</td>
		<td>对象的字符串表示</td>
	</tr>
    </tbody>
</table>

无需测试溢出（请见练习1.2.17），只需使两个long行实例变量表示分子和分母来控制溢出的可能性。使用欧几里德算法来保证分子和分母没有公因子。编写一个测试用力检测你实现的所有方法。

```java

public final class Rational{
	/**
	 * 分子
	 **/
	private long numerator;

	/**
	 * 分母
	 **/
	private long denominator;

	public Rational(long numerator, long denominator){
		assert numerator >= Integer.MIN_VALUE && numerator <= Integer.MAX_VALUE;
		assert denominator >= Integer.MIN_VALUE && numberator <= Integer.MAX_VALUE;

        if(numerator > 0 && denominator < 0 || numerator < 0 && denominator > 0){
            this.numerator = -Math.abs(numerator);
            this.denominator = Math.abs(denominator);
        }else{
		    this.numerator = Math.abs(numerator);
            this.denominator = Math.abs(denominator);
        }
		this.simplify();
	}

	private void simplify(){
		long a = Math.abs(this.numerator > this.denominator ? this.numerator : this.denominator);
		long b = Math.abs(this.numerator > this.denominator ? this.denominator : this.numerator);
		long c = a % b;
		while(c != 0){
			a = b;
			b = c;
			c = a % b;
		}

		this.numerator = this.numerator / b;
		this.denominator = this.denominator / b;
	}

	public Rational plus(Rational b){
		long numerator = this.numerator * b.denominator + b.numerator * this.denominator;
		long denominator = this.denominator * b.denominator;
		return new Rational(numerator, denominator);
	}

	public Rational minus(Rational b){
		long numerator = this.numerator * b.denominator - b.numerator * this.denominator;
		long denominator = this.denominator * b.denominator;
		return new Rational(numerator, denominator);
	}

	public Rational divides(Rational b){
		long numerator = this.numerator * b.denominator;
		long denominator = this.denominator * b.numerator;
		return new Rational(numerator, denominator);
	}

	public Rational times(Rational b){
		long numerator = this.numerator * b.numerator;
		long denominator = this.denominator * b.denominator;
		return new Rational(numerator, denominator);
	}

	public boolean equals(Rational that){
		if(that == null)
			return false;
		if(this == that)
			return true;
		return this.numerator == that.numerator && this.denominator == that.denominator;
	}

	@Override
	public String toString(){
		return String.format("%d/%d", this.numerator, this.denominator);
	}

	public static void main(String[] args){
		Rational a = new Rational(7, 12);
		Rational b = new Rational(10, 16);
		
		Rational c = a.plus(b);
		assert c.equals(new Rational(29, 24));

		c = a.minus(b);
        assert c.equals(new Rational(-1, 24));
        assert c.equals(new Rational(1, -24));

		c = a.times(b);
		assert c.equals(new Rational(35, 96));

		c = a.divides(b);
		assert c.equals(new Rational(14, 15));

		assert "5/8".equals(b.toString());
	}
}

```

### 1.2.17 有理数实现的健壮性。在Rational（请见练习1.2.16）的开发中使用断言防止溢出。 

### 1.2.18 累加器的方差。以下代码为Accumulator类添加了var()和stddev()方法，它们计算了addDataValue()方法的参数的方差和标准差，验证这段代码。

```java
public class Accumulator{
	private double m;
	private double s;
	private int N;

	public void addDataValue(double x){
		N++;
		s = s + 1.0 * (N - 1) / N * (x - m) * (x - m);
		m = m + (x - m) / N;
	}

	public double mean(){
		return m;
	}

	public double var(){
		return s / (N - 1);
	}

	public double stddev(){
		return Math.sqrt(this.var());
	}
}
```

与直接对所有数据的平方求和的方法相比较，这种实现能更好地避免四舍五入产生的误差。

$x<sub>1</sub>: m = 0 + \frac{(x_1 - 0)}{1} = x<sub>1</sub>$<br/>
x<sub>2</sub>: m = x<sub>1</sub> + (x<sub>2</sub> - x<sub>1</sub>) / 2 = (x<sub>1</sub> + x<sub>2</sub>) / 2<br/>
x<sub>3</sub>: m = (x<sub>1</sub> + x<sub>2</sub>) / 2 + (x<sub>3</sub> - (x<sub>1</sub> + x<sub>2</sub>) / 2) / 3 = (x<sub>1</sub> + x<sub>2</sub> + x<sub>3</sub>) / 3<br/>

可以证明对于某个特定的n有m(n)=(x[1] + x[2] + ... + x[n]) / n<br/>
根据上式有：m(n+1) = m(n) + (x[n+1] - m(n)) / (n + 1)<br/>
即：m(n+1) = (x[1] + x[2] + ... + x[n]) / n + (x[n+1] - (x[1] + x[2] + ... + x[n]) / n) / (n + 1)<br />
化简后为：m(n+1) = (x[1] + x[2] + ... + x[n+1]) / n + 1<br/>
故 m 即为所有数的平均值<br/>
