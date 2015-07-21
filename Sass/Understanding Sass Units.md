# Understanding Sass Units #
[Hugo Giraudel](http://www.sitepoint.com/author/hgiraudel/)

# 理解 Sass 中的单位 #
[Hugo Giraudel](http://www.sitepoint.com/author/hgiraudel/)

I have written about [units in Sass](http://hugogiraudel.com/2013/09/03/use-lengths-not-strings/ "Use Lengths, Not Strings") in the past for my own blog, and not so long ago as a [CSS-Tricks Sass snippet](http://css-tricks.com/snippets/sass/correctly-adding-unit-number/ "Correctly Adding Unit to Number"), but given how often I notice confusion and misconception about the way units work in our beloved preprocessor, I thought a full length article would not be such a bad idea after all.

我过去为自己的博客写过 [Sass 中的单位](http://hugogiraudel.com/2013/09/03/use-lengths-not-strings/ "使用长度，而非字符")，不久前写了[ CSS-Tricks Sass snippet](http://css-tricks.com/snippets/sass/correctly-adding-unit-number/ "正确地给数字添加单位")。但鉴于我注意到单位在大家所钟爱的预处理器中的作用方式常常被误解和混淆概念，我觉得最终写这么一篇完整的文章会是个不错的主意。

After two years of writing and reading Sass code every day, I must say that snippets, articles and library code misusing Sass units are beyond counting. I have [been guilty of this myself](https://github.com/sass/sass/issues/533#issuecomment-21362165) in the past, and I have witnessed Sass core designers trying to teach developers to properly use Sass units more than once.

有着两年中天天阅读和书写 Sass 代码经验的我，必须得说代码段、文章和库对 Sass 单位的误用数不胜数。我过去[一直为此自责](https://github.com/sass/sass/issues/533#issuecomment-21362165)。而且我也多次目睹了 Sass 的核心设计师们尽力教开发者们来正确使用 Sass 单位。

This battle is obviously not over so let’s hope this article will be the *coup de grace*, in some way.

显然这场战斗还没有结束，希望这篇文章能在某些方面成为致命一击。

## Units in real life ##
## 现实生活中的单位 ##

Before considering code, I think it is important to understand how numbers and units behave in real life. You may not have noticed this yet, but there are very few numbers we use in our daily life that do not have a unit. When we say “speed limit is 50″, we implicitly mean “50 kilometers per hour”; in this case the unit is *km/h*. When we say “look, eggs! I’ll have a dozen of those”, the unit is *eggs*: a dozen of eggs. There are exceptions of course but most usages of numbers in our daily routine usually involve a unit.

我认为在考虑代码前，很有必要来理解下现实生活中的数字和单位是如何表现的。或许你还没有注意到，日常生活中很少有数字没有单位。当说“限速 50”时，我们隐式地指“50 千米每小时”；这里的单位是*千米/时*。当我们说“看，鸡蛋！我要一打”时，单位是*鸡蛋*：一打鸡蛋。当然也有例外，但是通常大多数我们日常对数字的使用都与单位相关。

Along the same lines, running mathematical operations on numbers with units follow some rules. For instance, there are reasons why an area of 5 meters per 5 meters gives 25 square meters (`m²`). Same for a cube of 1 meter side: it has a volume of 1m ^ 3 because 1 meter per 1 meter per 1 meter doesn’t give 3 meters, but 1 cubic meter.

类似的，对包含单位的数字的数学运算也遵循着一些规则。比如，之所以 5 米乘 5 米等于 25 平方米 (`m²`)是有原因的。对边长为 1 米的立方体来说也是这样：它的体积为 1 立方米是因为 1 米乘以 1 米乘以 1 米不等于 3 米，而是 1 立方米。

I like how [Chris Eppstein](https://github.com/sass/sass/issues/533#issuecomment-21363158) puts it:

> **2ft ^ 2 is 4 square feet (an area), not 4 ft (which would be a length). The correct thing to do in this calculation is let the units multiply along with the number. The easiest way to think how to properly do math with units is to think of a unit like it is an unknown quantity in algebra.  
> (2 * x) ^ 2 is 4 * x². If the units at the end of a calculation don’t cancel out to become the dimension you’re expecting, this is a good sign that a mathematical error was made.**

我喜欢 [Chris Eppstein](https://github.com/sass/sass/issues/533#issuecomment-21363158) 的描述方式：
> **2 英尺的平方等于 4 平方英尺（面积单位）而非 4 英尺（这是长度单位）。这个计算中正确的算法是让单位和数字一样相乘。理解如何正确进行带单位的数学运算的方式就是把单位当成代数中的一个未知数。  
> (2 * x) ^ 2 等于 4 * x²。如果单位在运算结束后没有达到预期的维度，那么很可能出现了计算错误。**

We also have to understand that some units are literally incompatible. For instance, what would the result of 2 bananas + 3 apples be? Well, it turns out we cannot compute anything more than *2 bananas + 3 apples*, because apples and bananas are not compatible units. On the other hand, the result of 1 meter + 10 centimeters gives 1.1 meter or 110 centimeters. This is because meters and centimeters are both part of the metric system and thus are compatible.

我们也必须明白一些单位是无法相容的。例如，2 个香蕉 + 3 个苹果等于什么？显然，我们无法计算 *2 个香蕉 + 3 个苹果*，因为苹果和香蕉是不相容的单位。另一方面，1 米 + 10 厘米 等于 1.1 米或 110 厘米。这是因为米和厘米都属于公制，因此它们是相容的。

Of course units do not have to be part of the metric system to be compatible with each others. For instance, light-years and inches are compatible units since they both serve the same purpose: measuring distances. Of course, there is no way an equation would involve both light-years and inches, since both units live at different edges of the distance units spectrum. Still, they are compatible.

能相容的单位当然不一定要是公制的。比如，光年和英寸是相容的，因为它们的目的一样：测量距离。因为两个单位属于距离单位系列的不同极端，当然绝不会有计算式同时包含光年和英寸。尽管如此，它们仍然是相容的。

## Units in Sass ##
## Sass 中的单位 ##

Now that we have fully grasped the way units work in real life, it’s time to tackle units in Sass. I hope I will not surprise you when I say that units in Sass behave exactly like units in real life. Some units are compatible, some are not. Two numbers with the same unit that are multiplied together will produce square units, and so on.

现在我们已经完全理解了单位在现实生活中的作用方式，该来解决 Sass 中的单位了。但愿我这么说时你不会惊讶：Sass 单位的运作方式和现实生活中的单位的一模一样。一些单位是相容的，一些不是。两个有着一样单位的数字相乘会产生平方单位，诸如此类。

Let’s deal with one thing at a time, and start with comparing units. Sass has a [table of compatible units](https://github.com/sass/sass/blob/31c6c531fabc1538d37db334b2cf8b976e289c3b/lib/sass/script/value/number.rb#L455-L483) which makes it possible to figure out whether two units are comparable and compatible or not. For instance, `turn` (turns) and `deg` (degrees) are compatible; `s` (seconds) and `pt` (points) are not.

让我们一个个来看，从单位的比较开始吧。Sass 有个[相容单位表](https://github.com/sass/sass/blob/31c6c531fabc1538d37db334b2cf8b976e289c3b/lib/sass/script/value/number.rb#L455-L483)，它指明了两个单位是否是可比较的、相容的。例如，`turn`（圆周？？？）和 `deg`（度）是相容的，而 `s`（秒）和 `pt`（磅）不是。

This means that `12px + 1in` is a perfectly valid operation that will result in `108px`. *Why 108px?* you ask. Because 1 inch equals 96 pixels, and the result of the operation is expressed in the first member’s unit, which in this case is pixels. If we had written `1in + 12px`, the result would have been `1.125in`.

这意味着 `12px + 1in` 是完全有效的操作，其结果为 `108px`。你会问：*为什么是 108px？*因为 1 英寸等于 96 像素，且运算结果的单位会以第一个数的为准，所以这里是像素。如果算式为 `1in + 12px`，那么结果会是 `1.125in`。

Now, square units. While it is true that multiplying two numbers with the same unit should produce square units, in practice Sass does not allow square units because they do not make sense in CSS. When such a case occurs, Sass simply throws an error like so:

> **4px\*px isn’t a valid CSS value.**

现在来看看平方单位。尽管有相同单位的两个数相乘应该得到平方单位，但因为平方单位在 CSS 中没有意义，所以实际上 Sass 并不允许平方单位。而出现这一情况时，Sass 会直接报出如下错误：  

> **4px\*px 不是有效的 CSS 值。**

It took me some time to understand that this message does not mean that we tried to execute `4px * px`. In the aforementioned error message, `px*px` means `px²`, so the message could as well be:

> **4px² isn’t a valid CSS value.**

我花了些时间才明白这条信息并不是说我们试着计算 `4px * px`。在上述错误信息中， `px*px` 意味着 `px²`，如此则该条信息也可描述成：  

> **4px² 不是有效的 CSS 值。**

It all makes sense: any value using `px²` as a unit, or basically any square unit for that matter, is not a valid CSS value. CSS does not support square units since it does not make any sense to have those. Now, just for the record, if you tried to run `4px * px`, Sass would throw a different error message:

> **Undefined operation: “4px times px”.**

这样就讲得通了：任何以 `px²` 或基本任何平方单位为单位的值，都不是有效的 CSS 值。CSS 不支持平方单位是因为对它来说平方单位并没有意义。顺便说下，如果尝试运行 `4px * px`，Sass 会报不同于上面的错误信息：  

> **未定义的操作：“4px 乘以 px”。**

## What’s the matter? ##
## 有什么关系？ ##

Okay, so this has already been pretty long and perhaps you still don’t know what is the point of all that. This is the moment where I make it, don’t worry.

好了，在讲了这么多之后，你或许仍不明白这些都有什么意义。别担心，现在我就会说到。

When you have a unitless number, and you want to convert it to a specific unit, do not append the unit as a string. Please, I am begging you to stop this nonsense. A unit should always be coupled with a numeric value as it doesn’t make any sense in itself. `cm` is not a unit per se, it is a string; `1cm` is a unit, if that makes any sense.

当你想把一个无单位的数转为有单位的时，别把单位作为字符串加上。请千万不要如此胡闹。单位本身是没有意义的，所以单位总应该和数值一起使用。`cm` 本身不是单位，而是字符串；`1cm` 才是个单位。

When applying logical arithmetic principles to Sass, multiplying our value by 1 item of the desired unit is enough.

在 Sass 中运用逻辑运算法则时，用值为 1 的目标单位来乘以我们的数值就足够了。

	$value: 42;
	
	.foo {
		font-size: ($value * 1px); // {number} 42px
	}

Now you probably ask yourself *what’s the problem with simply appending the unit as a string given it works the same* and you would you be plain right in most scenarios. Now consider this:

现在你很可能会暗自思忖：*既然直接把字符串形式的单位加上去能起到一样的作用，那这有什么问题吗？*多数情情况下你都是完全正确的。现在考虑下这个：

	$value: 13.37;
	
	.foo {
		font-size: round($value + em); // {string} 13.37em
	}

Can you guess what will happen if we tried compiling this code? If your answer is `13em`, I am sorry to inform you that you’re wrong. The correct answer is:

> **$value: “13.37em” is not a number for `round’**

你能猜到编译这段代码会发生什么吗？如果你的答案是 `13em`，那么很遗憾地告诉你你错了。正确的回答是：

> **$value: “13.37em” is not a number for `round’**

This happens because simply adding the unit on the right of a number implicitly casts the value to a string, resulting in an incorrect parameter for the `round` function (which expects a number).

如此的原因是直接把单位加到数值的右边会隐式地把值转为字符串，导致其成为 `round` 函数的非法参数（合法参数为数值）。

Of course if you don’t plan on running extra mathematical operations on the new value, you will never notice the difference and neither will CSS. Yet, it seems quite poor to work like this with units when you know how clever Sass is with them. For the sake of consistency, elegance and meaningful code, I highly recommend that you properly convert your unitless numbers.

当然，如果你并不打算对新的值进行额外的运算操作，那么你和 CSS 都不会注意到其中的不同。然而，当你知道 Sass 能如此聪明地处理单位时，你也会发现继续那么做会显得很业余。为了一致性和优美且有意义的代码，我强烈推荐你恰当地转换无单位的数字。

Actually, it all makes sense when you turn the problem upside down and want to remove the unit of a number. If the solution that immediately comes to your mind is something such as:

	$value: 42px;
	$unitless-value: str-slice($value + ', 1, 2); // {string} 42
… then I am afraid you are doing this wrong. Removing the unit is as simple as dividing by one member of the unit. Again, it’s like in real life. If you want to get 25 from 25m, you have to divide 25m per 1m.

	$value: 42px;
	$unitless-value: (42px / 1px); // {number} 42
Then we have a number, not a string. It is completely logical, elegant and prevent any weird behaviour that might be surprising.

## The case of 0 ##

Before leaving, there is one thing I would like to extrapolate on. Earlier in this article, you may have read that the result of an addition or subtraction between two numbers of different units is expressed in the first member’s unit.

We can use this to our advantage when we want to convert one unit to another (compatible) unit for a reason or another. For instance, let’s say you want to convert seconds to milliseconds. I suppose you could multiply the value by 1000 in order to have milliseconds. On the other hand, you could leave this to Sass by adding the value to 0 millisecond, like so:

	$value: 42s;
	transition-duration: (0ms + $value); // {number} 42000ms
For such a case, I agree that we could as well compute the value ourselves. But when it comes to translating degrees into radians, or dots per centimeters (`dpcm`) to dot per pixels (`dppx`), it might be better to leave it to Sass rather than engage some fairly complex calculations.

	$resolution: 0dppx + 42dpcm; // {string} 1587.40157dppx
	$angle: 0rad + 42deg;        // {string} 0.73304rad
If you are afraid that this little trick looks too “hacky” to be used as is, you can create a short function to make the API a bit more friendly. I am not sure whether this is worth the trouble, but I’ll let you be the judge of that. Here is my take on this function:

	/// Convert one unit into another
	/// @author Hugo Giraudel
	/// @param {Number} $value - Initial value
	/// @param {String} $unit - Desired unit
	/// @return {Number}
	/// @throw Error if `$unit` does not exist or if units are incompatible.
	@function convert-unit($value, $unit) {
	  $units: (
	    'px': 0px,
	    'cm': 0cm,
	    'mm': 0mm,
	    '%': 0%,
	    'ch': 0ch,
	    'in': 0in,
	    'em': 0em,
	    'rem': 0rem,
	    'pt': 0pt,
	    'pc': 0pc,
	    'ex': 0ex,
	    'vw': 0vw,
	    'vh': 0vh,
	    'vmin': 0vmin,
	    'vmax': 0vmax,
	    'deg': 0deg,
	    'turn': 0turn,
	    'rad': 0rad,
	    'grad': 0grad,
	    's': 0s,
	    'ms': 0ms,
	    'Hz': 0Hz,
	    'kHz': 0kHz,
	    'dppx': 0dppx,
	    'dpcm': 0dpcm,
	    'dpi': 0dpi,
	  );
	 
	  @if map-has-key($units, $unit) {
	    @return map-get($units, $unit) + $value;
	  }
	 
	  @error "Unknown unit `#{$unit}`.";
	}
Rewriting our previous example:

	$resolution: convert-unit(42dpcm, 'dppx'); // {string} 1587.40157dppx
	$angle: convert-unit(42deg, 'rad');        // {string} 0.73304rad
## Final thoughts ##

So if we sum up what we’ve seen in this article:

- Units in Sass work exactly the same as units in real life;
- To give a number a unit, multiply the value per one member of the desired unit (e.g. `42 * 1px`);
- To remove unit from a number, divide by one member of the relevant unit (e.g. `42px / 1px`);
- When adding or subtracting two numbers with different compatible units, result is expressed in the unit of the first member;
- To convert a value from one unit to another (compatible), add the value to 0 member of the final unit (e.g. `0px + 42in`).

That’s it. I hope we’ll see less and less unit aberrations for more elegant unit handling from now on. As Chris says:

> **I think Sass’s math is really nice, once the light bulb goes on!**




原文地址： http://www.sitepoint.com/understanding-sass-units/