<div dir="rtl">

# الكود النظيف لجافاسكريبت

يمكنك قراءة المقالة بتنسيق أفضل على الرابط التالي:
[مدونة عبدالهادي الأندلسي](https://blog.abdelhadi.org/clean-code-javascript-in-arabic/)

## جدول المحتويات

1. [المقدمة](#المقدمة)
2. [المتغيرات](#المتغيرات)
3. [الدوال](#الدوال)
4. [الكائنات وهياكل البيانات](#الكائنات-وهياكل-البيانات)
5. [الأصناف](#الأصناف)
6. [مبادئ SOLID](#مبادئ-SOLID)
7. [الاختبار](#الاختبار)
8. [التزامن](#التزامن)
9. [التعامل مع الأخطاء](#التعامل-مع-الأخطاء)
10. [التنسيق](#التنسيق)
11. [التعليقات](#التعليقات)
12. [الترجمة](#الترجمة)
	
## المقدمة

![صورة طريفة لتقييم جودة البرمجيات بعدد الشتائم التي تصرخ بها عند قراءة الكود](https://www.osnews.com/images/comics/wtfm.jpg)

هذا دليل عن مبادئ هندسة البرمجيات، مستوحىً من كتاب روبرت سي مارتن الشهير المسمى "Clean Code" الذي يعني "الكود النظيف"، كُيفت هذه المبادئ لتتناسب مع لغة جافاسكريبت. الهدف منها هو توفير دليل عملي لمبرمجي جافاسكريبت لإنتاج برمجيات [مقروءة، وقابلة لإعادة الاستخدام (reusable)، وقابلة لإعادة هيكلتها (refactorable)](https://github.com/ryanmcdermott/3rs-of-software-architecture).

المبادئ المذكورة هنا إرشادات عامة لا أكثر، وليست قوانينا صارمة يُشترط اتباعها بحذافيرها. هي ليست محل اتفاق بين الجميع، إلا أنها نتاج خبرة جماعية على مدار سنوات لمؤلفي كتاب _الكود النظيف_.

هندسة البرمجيات فتية نسبيا إذ لا يتجاوز عمرها الخمسين سنة إلا بسنوات قليلة، عندما تكون معمارية البرمجيات قديمة قِدَم الهندسة المعمارية ذاتها، ربما حينئذٍ سيكون لدينا قواعد أكثر صرامة يجب علينا اتباعها، أما الآن فلا يزال أمامنا الكثير لنتعلمه، لكن يمكن التعامل مع هذه الإرشادات معيارًا لتقييم جودة كود جافاسكريبت الذي تكتبه أنت وفريقك.

هناك شيء آخر: معرفة هذه الإرشادات لن تجعلك على الفور مبرمجًا أفضل، ولن يجعلك تطبيقها لسنوات عديدة عصيًا على الأخطاء. يبدأ كل جزء من الكود بمسودة أولى، كالطين الرطب الذي ما زال يتشكّل في أطواره الأولى، ثم أخيرًا نزيل العيوب عند مراجعتها مع أقراننا. لا تشعر بالذنب حيال المسودات الأولى التي تحتاج إلى تحسين، وإنما ابذل وسعك في تحسين الكود بدًلا من ذلك.

## المتغيرات
	
### استخدم أسماء متغيرات معبّرة وسهلة النطق

**خطأ:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**صواب:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### استخدم نفس المفردات لنفس نوع المتغيرات

**خطأ:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**صواب:**

```javascript
getUser();
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### استخدم أسماء قابلة للبحث

سنقرأ الأكواد أكثر مما نكتبها. من المهم أن يكون الكود الذي نكتبه مقروءًا وقابلًا للبحث. عندما _لا نلتزم_ بكتابة متغيرات ذات معان لفهم برنامجنا، فنحن نؤذي قرّاءنا. اجعل أسماءك قابلة للبحث. ستساعدك أدوات مثل [buddy.js](https://github.com/danielstjules/buddy.js) و[ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md) على التعرف على الثوابت غير المسمّاة.
**خطأ:**

```javascript
// ماذا تعني هذه 86400000؟!
setTimeout(blastOff, 86400000);
```

**صواب:**

```javascript
// عرّف محتوى هذه الأرقام بهيئة ثوابت بحروف إنجليزية كبيرة
const MILLISECONDS_PER_DAY = 60 * 60 * 24 * 1000; //86400000;

setTimeout(blastOff, MILLISECONDS_PER_DAY);
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### استخدم متغيرات توضيحية

**خطأ:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
	address.match(cityZipCodeRegex)[1],
	address.match(cityZipCodeRegex)[2]
);
```

**صواب:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### تجنّب الإبهام

التصريح خير من التلميح.

**خطأ:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach((l) => {
	doStuff();
	doSomeOtherStuff();
	// ...
	// ...
	// ...
	// لحظة، ماذا تعني `l` مجددًا؟
	dispatch(l);
});
```

**صواب:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach((location) => {
	doStuff();
	doSomeOtherStuff();
	// ...
	// ...
	// ...
	dispatch(location);
});
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### لا داعي لتكرار الأسماء دون حاجة

إذا كان الصنف (class) أو الكائن (object) يعبّر عن معنى، لا تكرر هذا المعنى في متغيراته الداخلية.

**خطأ:**

```javascript
const Car = {
	carMake: "Honda",
	carModel: "Accord",
	carColor: "Blue",
};

function paintCar(car, color) {
	car.carColor = color;
}
```

**صواب:**

```javascript
const Car = {
	make: "Honda",
	model: "Accord",
	color: "Blue",
};

function paintCar(car, color) {
	car.color = color;
}
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### استخدم معاملات (parameters) افتراضية، وتجنب تعيين القيم الافتراضية من خلال الجمل الشرطية

المعاملات الافتراضية غالبًا أوضح من طريقة التعيين عن طريقة المعاملات المنطقية (short-circuit evaluation).
كن واعيًا إذا استخدمت الإسناد عن طريق المعاملات المنطقية. ستقدم دالتك فقط قيمًا افتراضية للوسطاء (arguments) غير المعرّفة "undefined"، أما القيم الخاطئة (falsy values) فلن تُستبدل بقيمة افتراضية، مثل: `''` و `""` و `false` و `null` و `0` و `NaN`.

**خطأ:**

```javascript
function createMicrobrewery(name) {
	const breweryName = name || "Hipster Brew Co."; // short-circuit evaluation
	// ...
}
```

**صواب:**

```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
	// ...
}
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

## **الدوال**

### وسطاء الدوال (Function arguments) (وسيطيَن اثنين أو أقل في الحالة المثالية)

يعد تحديد مقدار معاملات الدالة أمرًا مهمًا للغاية؛ لأنه يجعل اختبار الدالة أسهل. يؤدي وجود أكثر من ثلاثة معاملات إلى فوضى هائلة بحيث يتعيّن عليك اختبار العديد من الحالات المختلفة لكل وسيط على حِدة.

الحالة المثالية لعدد الوسطاء (arguments) هو واحد أو اثنين، ويجب تجنّب ثلاثة وسطاء (arguments) إن أمكن. أي عدد أكبر من المعاملات يوجب دمجها.

عادة، إذا كان لديك أكثر من معامليَن، فإن دالتك تحاول فعل أكثر مما يجب. في الحالات التي لا يكون فيها الأمر كذلك، فإن تمرير الكائن معاملًا (بدلًا من المعاملات الكثيرة) يعد خيارًا صحيحًا.

نظرًا لأن جافاسكريبت تسمح لك بإنشاء كائنات على الفور دون الكثير من الأكواد الإضافية للأصناف، فيمكنك استخدام كائن إذا وجدت نفسك بحاجة إلى الكثير من الوسطاء.

لتوضيح الخصائص التي تتوقعها الدالة، يمكنك استخدام تعبير التفكيك (destructuring) في ES2015/ES6. هذا له بعض المزايا:

1. عندما ينظر شخص ما إلى بُنية الدالة، يتضح على الفور ما الخصائص التي المستخدمة.
2. يمكن استخدامها لمحاكاة المعاملات المسماة (named parameters).
3. تؤدي عملية التفكيك (destructuring) أيضًا إلى نسخ القيم الأولية المحددة للكائن الذي يُمرّر إلى الدالة. هذا يمكن أن يساعد في منع الآثار الجانبية. ملاحظة: **لا تُنسخ** الكائنات والمصفوفات التي ُتفكّك من الكائن الذي يُمرر إلى الدالة.
4. يمكن أن تحذرك المنسّقات الآلية (Linters) من الخصائص (properties) غير المستخدمة، والتي ستكون مستحيلة دون عملية التفكيك (destructuring).

**خطأ:**

```javascript
function createMenu(title, body, buttonText, cancellable) {
	// ...
}

createMenu("Foo", "Bar", "Baz", true);
```

**صواب:**

```javascript
function createMenu({ title, body, buttonText, cancellable }) {
	// ...
}

createMenu({
	title: "Foo",
	body: "Bar",
	buttonText: "Baz",
	cancellable: true,
});
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### يجب أن تؤدي الدوال شيئًا واحدًا

هذه هي القاعدة الأهم في هندسة البرمجيات. عندما تقوم الدوال بأكثر من شيء، يصعب إنشاؤها واختبارها وفهمها.
عندما تتمكن من عزل كل إجراء داخل دالة واحدة فقط، يمكن إعادة هيكلتها بسهولة وسيتضح الكود أكثر. إن كان هذا الشيء الوحيد الذي تعلمته في هذا الدليل، فستتقدم على العديد من المطورين.

**خطأ:**

```javascript
function emailClients(clients) {
	clients.forEach((client) => {
		const clientRecord = database.lookup(client);
		if (clientRecord.isActive()) {
			email(client);
		}
	});
}
```

**صواب:**

```javascript
function emailActiveClients(clients) {
	clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
	const clientRecord = database.lookup(client);
	return clientRecord.isActive();
}
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### يجب أن تعبّر أسماء الدوال عمّا تفعله

**خطأ:**

```javascript
function addToDate(date, month) {
	// ...
}

const date = new Date();

/// من الصعب معرفة ما أُضيف من اسم الدالة
addToDate(date, 1);
```

**صواب:**

```javascript
function addMonthToDate(month, date) {
	// ...
}

const date = new Date();
addMonthToDate(1, date);
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### يجب أن تكون الدوال على مستوى واحد فقط من التجريد

عندما يكون لديك أكثر من مستوى واحد من التجريد، فإن الدالة عادة تفعل أكثر مما يجب. يؤدي تقسيم الدالة إلى دوال متعددة إلى قابلية أكبر لإعادة الاستخدام وسهولة أكبر عند الاختبار.

**خطأ:**

```javascript
function parseBetterJSAlternative(code) {
	const REGEXES = [
		// ...
	];

	const statements = code.split(" ");
	const tokens = [];
	REGEXES.forEach((REGEX) => {
		statements.forEach((statement) => {
			// ...
		});
	});

	const ast = [];
	tokens.forEach((token) => {
		// lex...
	});

	ast.forEach((node) => {
		// parse...
	});
}
```

**صواب:**

```javascript
function parseBetterJSAlternative(code) {
	const tokens = tokenize(code);
	const syntaxTree = parse(tokens);
	syntaxTree.forEach((node) => {
		// parse...
	});
}

function tokenize(code) {
	const REGEXES = [
		// ...
	];

	const statements = code.split(" ");
	const tokens = [];
	REGEXES.forEach((REGEX) => {
		statements.forEach((statement) => {
			tokens.push(/* ... */);
		});
	});

	return tokens;
}

function parse(tokens) {
	const syntaxTree = [];
	tokens.forEach((token) => {
		syntaxTree.push(/* ... */);
	});

	return syntaxTree;
}
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### احذف الكود المكرّر

ابذل قصارى جهدك لتجنب تكرار الكود. الكود المكرر سيء لأنه يعني أن هناك أكثر من مكان واحد لتغيير أي شيء كنت بحاجة إلى تغييره.

تخيل أنك تدير مطعمًا وتتابع مخزونك: كل الطماطم، والبصل، والثوم، والتوابل، وما إلى ذلك. إذا كانت لديك قوائم متعددة تحتفظ بها لهذه المكونات، فيجب تحديثها جميعًا عند تقديم طبق مع الطماطم. إذا كان لديك قائمة واحدة فقط، فهناك مكان واحد فقط لتحديثه!

في كثير من الأحيان يكون لديك كود مكرر لأن لديك شيئين مختلفين أو أكثر قليلاً، يشتركان في الكثير من القواسم المشتركة، لكن الاختلافات بينهما تجبرك على وجود دالتين منفصلتين أو أكثر تقومان بالكثير من الأشياء نفسها. إزالة كود مكرر يعني إنشاء تجريد يمكنه التعامل مع هذه المجموعة من الأشياء المختلفة بدالة أو وحدة أو صنف واحد فقط.

يُعد الإتيان بالتجريد الصحيح أمرًا بالغ الأهمية، ولهذا السبب يجب عليك اتباع مبادئ SOLID الموضحة في قسم [الأصناف](#الأصناف). يمكن أن تكون التجريدات السيئة أسوأ من الكود المكرر، لذا تَوَخَّ الحذر! بعد قولي هذا، إذا كان بإمكانك عمل تجريد جيد، فافعل ذلك! لا تكرر الكود، وإلا ستجد نفسك تحدّث أماكن متعددة في أي وقت تريد تغيير شيء واحد.

**خطأ:**

```javascript
function showDeveloperList(developers) {
	developers.forEach((developer) => {
		const expectedSalary = developer.calculateExpectedSalary();
		const experience = developer.getExperience();
		const githubLink = developer.getGithubLink();
		const data = {
			expectedSalary,
			experience,
			githubLink,
		};

		render(data);
	});
}

function showManagerList(managers) {
	managers.forEach((manager) => {
		const expectedSalary = manager.calculateExpectedSalary();
		const experience = manager.getExperience();
		const portfolio = manager.getMBAProjects();
		const data = {
			expectedSalary,
			experience,
			portfolio,
		};

		render(data);
	});
}
```

**صواب:**

```javascript
function showEmployeeList(employees) {
	employees.forEach((employee) => {
		const expectedSalary = employee.calculateExpectedSalary();
		const experience = employee.getExperience();

		const data = {
			expectedSalary,
			experience,
		};

		switch (employee.type) {
			case "manager":
				data.portfolio = employee.getMBAProjects();
				break;
			case "developer":
				data.githubLink = employee.getGithubLink();
				break;
		}

		render(data);
	});
}
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### عيّن الكائنات الافتراضية باستخدام Object.assign

**خطأ:**

```javascript
const menuConfig = {
	title: null,
	body: "Bar",
	buttonText: null,
	cancellable: true,
};

function createMenu(config) {
	config.title = config.title || "Foo";
	config.body = config.body || "Bar";
	config.buttonText = config.buttonText || "Baz";
	config.cancellable =
		config.cancellable !== undefined ? config.cancellable : true;
}

createMenu(menuConfig);
```

**صواب:**

```javascript
const menuConfig = {
	title: "Order",
	// لم يضف المستخدم مفتاح `body`
	buttonText: "Send",
	cancellable: true,
};

function createMenu(config) {
	let finalConfig = Object.assign(
		{
			title: "Foo",
			body: "Bar",
			buttonText: "Baz",
			cancellable: true,
		},
		config
	);
	return finalConfig;
	// الآن config تساوي: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
	// ...
}

createMenu(menuConfig);
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### لا تستخدم [متغيرات الراية (flags)](https://stackoverflow.com/questions/17402125/what-is-a-flag-variable) معاملات للدوال

تخبر متغيرات الراية (flags) المستخدم أن هذه الدالة تقوم بأكثر من شيء. يجب أن تعمل الدوال شيئًا واحدًا. قسّم دوالك إذا كانت تتبع مسارات كود مختلفة بناءً على قيمة منطقية.

**خطأ:**

```javascript
function createFile(name, temp) {
	if (temp) {
		fs.create(`./temp/${name}`);
	} else {
		fs.create(name);
	}
}
```

**صواب:**

```javascript
function createFile(name) {
	fs.create(name);
}

function createTempFile(name) {
	createFile(`./temp/${name}`);
}
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### تجنب التأثيرات الجانبية (side effects) (الجزء الأول)

تنتج الدالة تأثيرًا جانبيًا إذا فعلت أي شيء آخر غير أخذ قيمة وإرجاع قيمة أو قيم أخرى. قد يكون أحد التأثيرات الجانبية هو الكتابة على ملف، أو تعديل بعض المتغيرات العامة، أو تحويل كل أموالك بطريق الخطأ إلى شخص غريب.

الآن، أنت بالفعل بحاجة إلى أن يكون لديك تأثيرات جانبية في البرنامج في بعض الأحيان. مثل المثال السابق، قد تحتاج إلى الكتابة على ملف. ما تريد القيام به هو التركيز على مكان الآثار الجانبية. لا تستخدم العديد من الدوال والأصناف للكتابة على ملف، بل أنشئ خدمة (service) واحدة تفعل ذلك. خدمة واحدة فقط لا غير.

الأمر المهم هنا هو تجنب العثرات الشائعة مثل مشاركة الحالة بين الكائنات دون هيكلية واضحة، باستخدام أنواع بيانات قابلة للتغيير (mutable) يمكن أن يعدلها أي جزء من الكود. والصحيح جعلُ الحالة متمركزة حيث تحصل التأثيرات الجانبية. إذا تمكنت من القيام بذلك، فستكون أسعد من الغالبية العظمى من المبرمجين الآخرين.

**خطأ:**

```javascript
// متغير عام مشار إليه من قبل الدالة التالية.
// إذا كانت لدينا دالة أخرى تستخدم هذا الاسم، فستكون الآن مصفوفة ويمكن أن تعطلها.
let name = "Ryan McDermott";

function splitIntoFirstAndLastName() {
	name = name.split(" ");
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**صواب:**

```javascript
function splitIntoFirstAndLastName(name) {
	return name.split(" ");
}

const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### تجنب الآثار الجانبية (side effects) (الجزء الثاني)

في جافاسكريبت، بعض القيم غير قابلة للتغيير (immutable) وبعضها قابلة للتغيير (mutable). الكائنات والمصفوفات هما نوعان من القيم القابلة للتغيير لذلك من المهم التعامل معها بحذر عندما تُمرران معاملات للدالة. دالة جافاسكريبت يمكنها تغيير خاصيات الكائن أو تعديل محتوى المصفوفة مما قد يسبب أخطاء في أماكن أخرى.

لنفترض أن هناك دالة تقبل معاملًا من نوع مصفوفة يعبر عن سلة شراء. إذا كانت الدالة تغير تلك المصفوفة بإضافة عنصر للشراء مثلا، فإن أي دالة أخرى تستخدم نفس مصفوفة السلة `cart` ستكون متأثرة بهذه الإضافة. هذا قد يكون عظيمًا، ولكن قد يكون سيئًا أيضًا. لنتخيل حالة سيئة:

ينقر المستخدم على زر "شراء" والذي يستدعي دالة `purchase` التي تقدم طلبًا عبر الشبكة وترسل مصفوفة `cart` إلى الخادوم. بسبب ضعف اتصال الشبكة تظل دالة `purchase` تعيد الطلب. الآن، ماذا لو أن المستخدم في نفس الوقت نقر على زر "إضافة إلى السلة" بالخطأ على عنصر لا يريده قبل بدء طلب الشبكة؟ لو حدث ذلك وبدأ طلب الشبكة فإن دالة الشراء سترسل العنصر الذي أضيف بالخطأ لأن مصفوفة `cart` عُدّلت.
A great solution would be for the `addItemToCart` function to always clone the `cart`, edit it, and return the clone. This would ensure that functions that are still using the old shopping cart wouldn't be affected by the changes.

أحد الحلول المثلى سيكون أن دالة `addItemToCart` ستنسخ محتوى مصفوفة `cart` ، وتعدلها ثم ترجع النسخة. هذا الأمر سيضمن أن الدوال التي ما زالت تستخدم سلة الشراء القديمة لن تتأثر بالتغييرات.

هناك تنبيهان عند استخدام هذه الطريقة:

1. قد تكون هناك حالات عندما تكون بالفعل تريد تعديل كائن الإدخال، ولكن عندما تتبع هذه الممارسة البرمجية ستجد أن هذه الحالات نادرة. معظم الأشياء قابلة لإعادة الهيكلة لكي تكون دون آثار جانبية!

2. نسخ كائنات ضخمة قد يكون مؤثرًا على الأداء. لكن لحسن الحظ فهذه ليست مشكلة كبيرة لأن هناك [مكتبة عظيمة](https://facebook.github.io/immutable-js/) تسمح بهذا النوع من الأسلوب البرمجي أن يكون سريعًا ولا يؤثر على الذاكرة بعكس لو كنت تنسخ الكائنات والمصفوفات يدويًا.

**خطأ:**

```javascript
const addItemToCart = (cart, item) => {
	cart.push({ item, date: Date.now() });
};
```

**صواب:**

```javascript
const addItemToCart = (cart, item) => {
	return [...cart, { item, date: Date.now() }];
};
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### لا تعدّل دوال المجال العام (global functions)

يعد تعديل دوال المجال العام (global functions) ممارسة خاطئة في جافاسكريبت؛ لأنها قد تتعارض مع مكتبة أخرى، وسيختلط على المستخدم الذي يستخدم واجهتك البرمجية (API) فلن يعرف شيئًا حتى يلقى الاستثناء أو الخطأ في التشغيل (exception in production).

لنفكر بمثال: ماذا لو أردت تمديد دالة المصفوفات الأصلية في جافاسكريبت `Array` وأضفت دالة `diff` لتعطي الفرق بين مصفوفتين؟
يمكنك عمل دالتك عن طريق `Array.prototype` ولكن هذا قد يتعارض مع مكتبة أخرى أرادت عمل نفس الشيء. ماذا لو أن المكتبة استخدمت `diff` لمعرفة الفرق بين العنصر الأول والأخير في الدالة؟
لذلك هذا ما يجعل من الأفضل استخدام أصناف ES2015/ES6 وببساطة تمديد `Array` العامة.

**خطأ:**

```javascript
Array.prototype.diff = function diff(comparisonArray) {
	const hash = new Set(comparisonArray);
	return this.filter((elem) => !hash.has(elem));
};
```

**صواب:**

```javascript
class SuperArray extends Array {
	diff(comparisonArray) {
		const hash = new Set(comparisonArray);
		return this.filter((elem) => !hash.has(elem));
	}
}
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### فضّل البرمجة الدالّية (functional) على البرمجة الأمرية (imperative)

ليست جافاسكريبت لغة دالية كما هو الحال في لغة Haskell. ولكن لها بعض جوانب اللغات الدالّية. تكون اللغات الدالّية أوضح وأسهل في الاختبار. استخدم هذا الأسلوب البرمجي قدر ما تستطيع.

**خطأ:**

```javascript
const programmerOutput = [
	{
		name: "Uncle Bobby",
		linesOfCode: 500,
	},
	{
		name: "Suzie Q",
		linesOfCode: 1500,
	},
	{
		name: "Jimmy Gosling",
		linesOfCode: 150,
	},
	{
		name: "Gracie Hopper",
		linesOfCode: 1000,
	},
];

let totalOutput = 0;

for (let i = 0; i < programmerOutput.length; i++) {
	totalOutput += programmerOutput[i].linesOfCode;
}
```

**صواب:**

```javascript
const programmerOutput = [
	{
		name: "Uncle Bobby",
		linesOfCode: 500,
	},
	{
		name: "Suzie Q",
		linesOfCode: 1500,
	},
	{
		name: "Jimmy Gosling",
		linesOfCode: 150,
	},
	{
		name: "Gracie Hopper",
		linesOfCode: 1000,
	},
];

const totalOutput = programmerOutput.reduce(
	(totalLines, output) => totalLines + output.linesOfCode,
	0
);
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### غلّف الجمل الشرطية (Encapsulate conditionals)

**خطأ:**

```javascript
if (fsm.state === "fetching" && isEmpty(listNode)) {
	// ...
}
```

**صواب:**

```javascript
function shouldShowSpinner(fsm, listNode) {
	return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
	// ...
}
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### تجنّب الجمل الشرطية المنفية

**خطأ:**

```javascript
function isDOMNodeNotPresent(node) {
	// ...
}

if (!isDOMNodeNotPresent(node)) {
	// ...
}
```

**صواب:**

```javascript
function isDOMNodePresent(node) {
	// ...
}

if (isDOMNodePresent(node)) {
	// ...
}
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### تجنّب الجمل الشرطية

الجواب هو أنه يمكنك استخدام تعدد الأشكال (polymorphism) لتحقيق نفس المهمة في كثير من الحالات.
السؤال الثاني هو عادة، "حسنًا هذا رائع ولكن لماذا أرغب في القيام بذلك؟"
الجواب هو مفهوم الكود النظيف السابق الذي تعلمناه: يجب أن تقوم الدالة بشيء واحد فقط. عندما يكون لديك أصناف ودوال تحتوي على عبارات "if" الشرطية، فأنت تخبر المستخدم أن دالتك تقوم بأكثر من شيء. تذكر ، فقط افعل شيئًا واحدًا.

**خطأ:**

```javascript
class Airplane {
	// ...
	getCruisingAltitude() {
		switch (this.type) {
			case "777":
				return this.getMaxAltitude() - this.getPassengerCount();
			case "Air Force One":
				return this.getMaxAltitude();
			case "Cessna":
				return this.getMaxAltitude() - this.getFuelExpenditure();
		}
	}
}
```

**صواب:**

```javascript
class Airplane {
	// ...
}

class Boeing777 extends Airplane {
	// ...
	getCruisingAltitude() {
		return this.getMaxAltitude() - this.getPassengerCount();
	}
}

class AirForceOne extends Airplane {
	// ...
	getCruisingAltitude() {
		return this.getMaxAltitude();
	}
}

class Cessna extends Airplane {
	// ...
	getCruisingAltitude() {
		return this.getMaxAltitude() - this.getFuelExpenditure();
	}
}
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### تجنّب التحقق من أنواع البيانات (الجزء الأول)

لغة جافاسكريبت لغة لا تلتزم بنوع البيانات (untyped)، مما يعني أن الدوال يمكن أن تأخذ أي نوع من الوسطاء. أحيانًا تؤذيك هذه الحرية ويصبح من المغري التحقق من الأنواع في الدوال. هناك العديد من الطرق لتجنب الاضطرار إلى ذلك. أول شيء يجب مراعاته هو واجهات برمجية متسقة.

**خطأ:**

```javascript
function travelToTexas(vehicle) {
	if (vehicle instanceof Bicycle) {
		vehicle.pedal(this.currentLocation, new Location("texas"));
	} else if (vehicle instanceof Car) {
		vehicle.drive(this.currentLocation, new Location("texas"));
	}
}
```

**صواب:**

```javascript
function travelToTexas(vehicle) {
	vehicle.move(this.currentLocation, new Location("texas"));
}
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### تجنّب التحقق من أنواع البيانات (الجزء الثاني)

إذا كنت تعمل بأنواع البيانات الأساسية (primitive values) مثل سلاسل النصوص (string) والأعداد الصحيحة (integers)، ولا يمكنك استخدام تعدد الأشكال (polymorphism) ولكنك ما زلت تشعر بالحاجة إلى التحقق من الأنواع (type-check)، فعليك التفكير في استخدام TypeScript. إنها بديل ممتاز لجافاسكريبت العادية لأنها توفر لك إمكانية تعريف أنواع بيانات ساكنة للمتغيّرات (static typing) عدا عن كونها مبنية على صيغة جافاسكريبت القياسية.
تكمن مشكلة التحقق من الأنواع يدويًا في جافاسكريبت في أن القيام بذلك جيدًا يتطلب الكثير من الإسهاب الإضافي الذي لا يؤدي إلا إلى أمان زائف من تحقق الأنواع وتفقد مع ذلك مقروئية الكود (code readability).
حافظ على نظافة كود جافاسكريبت، واكتب اختبارات جيدة، واحصل على مراجعات جيدة للكود (code review). بخلاف ذلك، افعل كل ذلك باستخدام TypeScript، والتي كما قلت أنها بديل رائع.

**خطأ:**

```javascript
function combine(val1, val2) {
	if (
		(typeof val1 === "number" && typeof val2 === "number") ||
		(typeof val1 === "string" && typeof val2 === "string")
	) {
		return val1 + val2;
	}

	throw new Error("Must be of type String or Number");
}
```

**صواب:**

```javascript
function combine(val1, val2) {
	return val1 + val2;
}
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### لا تفرط في التحسين

تُحَسِّن المتصفحات الحديثة الكثير خلف الكواليس أثناء التشغيل. في كثير من الأحيان، إذا كنت تنشغل بالتحسين، فأنت تضيع وقتك فقط. [هناك مصادر جيدة](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers) لمعرفة الأماكن المفتقرة إلى التحسين. استهدفها هذه الأثناء، حتى تُصلح إذا أمكن ذلك.

**خطأ:**

```javascript
// في المتصفحات القديمة، سيكون كل تكرار مع "list.length" غير المخزنة مؤقتًا مؤثرًا على الأداء
// بسبب إعادة حساب "list.length". في المتصفحات الحديثة، هنا الأمر حُسّن أداؤه.
for (let i = 0, len = list.length; i < len; i++) {
	// ...
}
```

**صواب:**

```javascript
for (let i = 0; i < list.length; i++) {
	// ...
}
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### احذف الكود الميت

الكود الميت سيء بقدر الكود المكرر. لا يوجد سبب لإبقائه في مجموع الكود. إذا لم يكن يُستدعى فتخلص منه! سيظل آمنًا في محفوظات نظام التحكم في الإصدارات مثل git إذا احتجته.

**خطأ:**

```javascript
function oldRequestModule(url) {
	// ...
}

function newRequestModule(url) {
	// ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**صواب:**

```javascript
function newRequestModule(url) {
	// ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

## **الكائنات وهياكل البيانات**

### استخدم الجالبات (getters) والضوابط (setters)

استخدام الجالبات (getters) والضوابط (setters) للوصول إلى بيانات الكائن قد يكون أفضل من مجرد البحث عن خاصية (property) في الكائن. قد تسأل "لماذا؟" حسنًا، هاك قائمة من الأسباب:

- عندما تريد أن تعمل شيئًا ما يفوق مجرد الحصول على خاصية أحد الكائنات، لا يجب عليك البحث وتغيير كل استدعاء لها في أكوادك البرمجية.
- يسهل إضافة التحقق (validation) عند استخدام الضابط (set).
- يغلّف الدقائق التفصيلية للعرض.
- أسهل لإضافة سجل تتبع، والتعامل مع الأخطاء عند استخدام الجالبات (getters) والضوابط (setters).
- يمكنك التحميل البطيء (lazy loading) لخصائص الكائن، مثلًا، عند جلبها من الخادوم.

**خطأ:**

```javascript
function makeBankAccount() {
	// ...

	return {
		balance: 0,
		// ...
	};
}

const account = makeBankAccount();
account.balance = 100;
```

**صواب:**

```javascript
function makeBankAccount() {
	// هذه الخاصية خاصة
	let balance = 0;

	// الجالب جعلها عامة عن طريق الكائن الراجع في الأسفل
	function getBalance() {
		return balance;
	}

	// أما الضابط، جعلها عامة عن طريق الكائن الراجع في الأسفل
	function setBalance(amount) {
		//… التحقق قبل تحديث خاصة رصيد الحساب "balance"
		balance = amount;
	}

	return {
		// ...
		getBalance,
		setBalance,
	};
}

const account = makeBankAccount();
account.setBalance(100);
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### دع الكائنات تمتلك عناصر خاصة

يمكن إنجاز هذا عن طريق التعابير المغلقة (closures) في إصدار ES5 وما دونه.

**خطأ:**

```javascript
const Employee = function (name) {
	this.name = name;
};

Employee.prototype.getName = function getName() {
	return this.name;
};

const employee = new Employee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // اسم الموظف: John Doe

delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // اسم الموظف: غير معرّف "undefined"
```

**صواب:**

```javascript
function makeEmployee(name) {
	return {
		getName() {
			return name;
		},
	};
}

const employee = makeEmployee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // اسم الموظف: John Doe

delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // اسم الموظف: John Doe
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

## الأصناف

### فضّل أصناف ES2015/ES6 على دوال ES5 العادية

من الصعب جدًا الحصول على توريث صنف (class inheritance) ذي مقروئية، والحصول على دالة البناء (construction)، وتعريف الدوال في أصناف ES5 التقليدية. لو كنت تريد التوريث (وكن متيقظًا أنك قد لا تحتاجه بالأساس)، فاختر أصناف ES2015/ES6. ومع ذلك، فالأفضل لك الدوال الصغيرة على الأصناف حتى تجد نفسك بحاجة إلى كائنات أكبر وأعقد.

**خطأ:**

```javascript
const Animal = function (age) {
	if (!(this instanceof Animal)) {
		throw new Error("Instantiate Animal with `new`");
	}

	this.age = age;
};

Animal.prototype.move = function move() {};

const Mammal = function (age, furColor) {
	if (!(this instanceof Mammal)) {
		throw new Error("Instantiate Mammal with `new`");
	}

	Animal.call(this, age);
	this.furColor = furColor;
};

Mammal.prototype = Object.create(Animal.prototype);
Mammal.prototype.constructor = Mammal;
Mammal.prototype.liveBirth = function liveBirth() {};

const Human = function (age, furColor, languageSpoken) {
	if (!(this instanceof Human)) {
		throw new Error("Instantiate Human with `new`");
	}

	Mammal.call(this, age, furColor);
	this.languageSpoken = languageSpoken;
};

Human.prototype = Object.create(Mammal.prototype);
Human.prototype.constructor = Human;
Human.prototype.speak = function speak() {};
```

**صواب:**

```javascript
class Animal {
	constructor(age) {
		this.age = age;
	}

	move() {
		/* ... */
	}
}

class Mammal extends Animal {
	constructor(age, furColor) {
		super(age);
		this.furColor = furColor;
	}

	liveBirth() {
		/* ... */
	}
}

class Human extends Mammal {
	constructor(age, furColor, languageSpoken) {
		super(age, furColor);
		this.languageSpoken = languageSpoken;
	}

	speak() {
		/* ... */
	}
}
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### استخدام سَلسَلة الدوال

هذا النمط مفيد جدًا في جافاسكريبت، ويمكنك رؤيته في العديد من المكتبات مثل jQuery و Lodash. يسمح لكودك أن يكون معبرًا، وأقل فوضى. لهذا، استخدم سَلسَلة الدوال وألق نظرة على مدى نظافة الكود. في دوال الصنف، ما عليك سوى إرجاع `this` في نهاية كل دالة، ويمكنك ربط المزيد من دوال الصنف بها.

**خطأ:**

```javascript
class Car {
	constructor(make, model, color) {
		this.make = make;
		this.model = model;
		this.color = color;
	}

	setMake(make) {
		this.make = make;
	}

	setModel(model) {
		this.model = model;
	}

	setColor(color) {
		this.color = color;
	}

	save() {
		console.log(this.make, this.model, this.color);
	}
}

const car = new Car("Ford", "F-150", "red");
car.setColor("pink");
car.save();
```

**صواب:**

```javascript
class Car {
	constructor(make, model, color) {
		this.make = make;
		this.model = model;
		this.color = color;
	}

	setMake(make) {
		this.make = make;
		// ملاحظة: إرجاع `this` للسلسلة
		return this;
	}

	setModel(model) {
		this.model = model;
		// ملاحظة: إرجاع `this` للسلسلة
		return this;
	}

	setColor(color) {
		this.color = color;
		// ملاحظة: إرجاع `this` للسلسلة
		return this;
	}

	save() {
		console.log(this.make, this.model, this.color);
		// ملاحظة: إرجاع `this` للسلسلة
		return this;
	}
}

const car = new Car("Ford", "F-150", "red").setColor("pink").save();
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### فضّل أسلوب التركيب (composition) على التوريث (inheritance)

كما ذُكر في كتاب [Design Patterns](https://en.wikipedia.org/wiki/Design_Patterns) الذي ألّفه أربعة من علماء الحاسوب عُرفوا باسم "عصابة الأربعة"، يجب أن تفضّل التركيب (composition) على التوريث (inheritance) ما أمكنك ذلك. هناك الكثير من الأسباب الوجيهة لاستخدام التوريث والعديد من الأسباب الوجيهة كذلك لاستخدام التركيب.

النقطة الأساسية في هذا المبدأ هي أنه إذا ذهب عقلك بداهة إلى التوريث، فحاول التفكير فيما إذا كان التركيب يمكن أن يصوغ مشكلتك صياغة أفضل. في بعض الحالات يمكن ذلك.

قد تتساءل: "متى يمكنني استخدام التوريث؟" يعتمد الأمر على مشكلتك، ولكن هذه قائمة جيدة عندما يكون التوريث أكثر منطقية من التركيب:

1. يمثل التوريث علاقة "is-a" (التعريف) وليس علاقة "has-a" (الامتلاك) (الإنسان-> الحيوان (الإنسان هو حيوان، أي ينتمي للمملكة الحيوانية) مقابل المستخدم-> تفاصيل المستخدم (حساب المستخدم يمتلك تفاصيل)).
2. يمكنك إعادة استخدام الأكواد من الأصناف الأساسية (يمكن للبشر التحرك مثل جميع الحيوانات).
3. تريد إجراء تغييرات عامة على الأصناف المشتقة عن طريق تغيير الصنف الأساسي. (تغيير إنفاق السعرات الحرارية لجميع الحيوانات عندما تتحرك).

**خطأ:**

```javascript
class Employee {
	constructor(name, email) {
		this.name = name;
		this.email = email;
	}

	// ...
}

// سيئ لأن الموظفين "Employees" "لديهم" بيانات ضريبية. أما بيانات الضريبة للموظفين EmployeeTaxData ليس نوعًا من أنواع الموظفين
class EmployeeTaxData extends Employee {
	constructor(ssn, salary) {
		super();
		this.ssn = ssn;
		this.salary = salary;
	}

	// ...
}
```

**صواب:**

```javascript
class EmployeeTaxData {
	constructor(ssn, salary) {
		this.ssn = ssn;
		this.salary = salary;
	}

	// ...
}

class Employee {
	constructor(name, email) {
		this.name = name;
		this.email = email;
	}

	setTaxData(ssn, salary) {
		this.taxData = new EmployeeTaxData(ssn, salary);
	}
	// ...
}
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

## مبادئ **SOLID**

### مبدأ المسؤولية الواحدة أو الفردية Single Responsibility Principle (SRP)

كما ذُكر في كتاب الكود النظيف، "يجب ألا يكون هناك أكثر من سبب لتغيير الصنف". من المغري أن تملأ صنفًا بالكثير من الدوال، كما تحزم حقيبة سفر واحدة فقط على متن رحلتك. تكمن المشكلة في أن الصنف لن يكون متماسكًا مفاهيميًا وسيكون أكثر عرضة للتغيير للعديد من الأسباب. من المهم تقليل عدد المرات التي تحتاج فيها لتغيير الصنف. إنه أمر مهم لأنه إذا كان هناك الكثير من الدوال في صنف واحد وعدّلت جزءًا منها، فقد يكون من الصعب فهم كيف سيؤثر ذلك على الوحدات التابعة الأخرى في مجموع الكود.

**خطأ:**

```javascript
class UserSettings {
	constructor(user) {
		this.user = user;
	}

	changeSettings(settings) {
		if (this.verifyCredentials()) {
			// ...
		}
	}

	verifyCredentials() {
		// ...
	}
}
```

**صواب:**

```javascript
class UserAuth {
	constructor(user) {
		this.user = user;
	}

	verifyCredentials() {
		// ...
	}
}

class UserSettings {
	constructor(user) {
		this.user = user;
		this.auth = new UserAuth(user);
	}

	changeSettings(settings) {
		if (this.auth.verifyCredentials()) {
			// ...
		}
	}
}
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### مبدأ الفتح والإغلاق Open/Closed Principle (OCP)

كما ذكر برتراند ماير، "يجب أن تكون الكيانات البرمجية (مثل الأصناف، والوحدات، والدوال، إلخ) مفتوحة للتوسع والتمديد، ولكن مغلقة أمام التعديل."
ماذا يعني ذلك؟ يعني ببساطة أنه يجب عليك السماح للمستخدمين إضافة وظائف جديدة دون تغيير الكود الموجود.

**خطأ:**

```javascript
class AjaxAdapter extends Adapter {
	constructor() {
		super();
		this.name = "ajaxAdapter";
	}
}

class NodeAdapter extends Adapter {
	constructor() {
		super();
		this.name = "nodeAdapter";
	}
}

class HttpRequester {
	constructor(adapter) {
		this.adapter = adapter;
	}

	fetch(url) {
		if (this.adapter.name === "ajaxAdapter") {
			return makeAjaxCall(url).then((response) => {
				// تحويل الاستجابة والإرجاع
			});
		} else if (this.adapter.name === "nodeAdapter") {
			return makeHttpCall(url).then((response) => {
				// تحويل الاستجابة والإرجاع
			});
		}
	}
}

function makeAjaxCall(url) {
	//  الطلب (request) وإرجاع الوعد (return promise)
}

function makeHttpCall(url) {
	//  الطلب (request) وإرجاع الوعد (return promise)
}
```

**صواب:**

```javascript
class AjaxAdapter extends Adapter {
	constructor() {
		super();
		this.name = "ajaxAdapter";
	}

	request(url) {
		//  الطلب (request) وإرجاع الوعد (return promise)
	}
}

class NodeAdapter extends Adapter {
	constructor() {
		super();
		this.name = "nodeAdapter";
	}

	request(url) {
		//  الطلب (request) وإرجاع الوعد (return promise)
	}
}

class HttpRequester {
	constructor(adapter) {
		this.adapter = adapter;
	}

	fetch(url) {
		return this.adapter.request(url).then((response) => {
			//  الطلب (request) وإرجاع الوعد (return promise)
		});
	}
}
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### مبدأ ليسكوف للاستبدال‎‏ ‎‎Liskov Substitution Principle ‎(LSP)‎‏‏‏

هذا مصطلح مخيف لمفهوم سهل للغاية. يُعّرف "رسميًا" على أنه "إذا كان S نوعًا فرعيًا من T، فيمكن استبدال كائنات من النوع T بكائنات من النوع S (مثلًا، قد تحل كائنات من النوع S محل كائنات من النوع T) دون تغيير أي من الخصائص المرغوبة لهذا البرنامج (الصحة، الأداء ، إلخ). " هذا تعريف أكثر ترويعًا!

أفضل شرح لهذا المفهوم هو إذا كان لديك صنف أساسي وصنف فرعي، فيمكن استخدام الصنف الأساسي والصنف الفرعي بالتبادل دون الحصول على نتائج غير صحيحة. قد يكون هذا محيرًا، لذلك دعونا نلقي نظرة على مثال "المربع-المستطيل" Square-Rectangle الكلاسيكي. من الناحية الرياضية، يعد المربع مستطيلًا، ولكن إذا نمذجته باستخدام علاقة التعريف "is-a" عبر التوريث ، فإنك ستقع في مشكلة بسرعة.

**خطأ:**

```javascript
class Rectangle {
	constructor() {
		this.width = 0;
		this.height = 0;
	}

	setColor(color) {
		// ...
	}

	render(area) {
		// ...
	}

	setWidth(width) {
		this.width = width;
	}

	setHeight(height) {
		this.height = height;
	}

	getArea() {
		return this.width * this.height;
	}
}

class Square extends Rectangle {
	setWidth(width) {
		this.width = width;
		this.height = width;
	}

	setHeight(height) {
		this.width = height;
		this.height = height;
	}
}

function renderLargeRectangles(rectangles) {
	rectangles.forEach((rectangle) => {
		rectangle.setWidth(4);
		rectangle.setHeight(5);
		const area = rectangle.getArea(); //خطأ: يرجع 25 للمربع، والصحيح هو 20
		rectangle.render(area);
	});
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**صواب:**

```javascript
class Shape {
	setColor(color) {
		// ...
	}

	render(area) {
		// ...
	}
}

class Rectangle extends Shape {
	constructor(width, height) {
		super();
		this.width = width;
		this.height = height;
	}

	getArea() {
		return this.width * this.height;
	}
}

class Square extends Shape {
	constructor(length) {
		super();
		this.length = length;
	}

	getArea() {
		return this.length * this.length;
	}
}

function renderLargeShapes(shapes) {
	shapes.forEach((shape) => {
		const area = shape.getArea();
		shape.render(area);
	});
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### مبدأ فصل الواجهات Interface Segregation Principle (ISP)

لا تحتوي جافاسكريبت على واجهات (interfaces)، لذا فإن هذا المبدأ لا ينطبق انطباقًا صارمًا كالمبادئ الأخرى. ومع ذلك، فهو مبدأ مهم وذو صلة حتى مع افتقار جافاسكريبت إلى نظام أنواع البيانات (type system).

ينص مبدأ فصل الواجهات على أنه "لا ينبغي إجبار العملاء على الاعتماد على الواجهات (interfaces) التي لا يستخدمونها." الواجهات هي عقود ضمنية في JavaScript بسبب نظام التحقق من الأنواع.

من الأمثلة الجيدة التي تستحق النظر إليها والتي توضح هذا المبدأ في جافاسكريبت هي الأصناف التي تتطلب كائن إعدادات كبير. يُعد عدم مطالبة العملاء بإعداد كميات هائلة من الخيارات أمرًا مفيدًا؛ لأنهم في معظم الأوقات لن يحتاجوا إلى جميع الإعدادات. يساعد جعلها اختيارية على منع وجود واجهة متخمة (fat interface).

**خطأ:**

```javascript
class DOMTraverser {
	constructor(settings) {
		this.settings = settings;
		this.setup();
	}

	setup() {
		this.rootNode = this.settings.rootNode;
		this.settings.animationModule.setup();
	}

	traverse() {
		// ...
	}
}

const $ = new DOMTraverser({
	rootNode: document.getElementsByTagName("body"),
	animationModule() {}, // في معظم الوقت، لن نحتاج إلى التحريك أثناء العبور
	// ...
});
```

**صواب:**

```javascript
class DOMTraverser {
	constructor(settings) {
		this.settings = settings;
		this.options = settings.options;
		this.setup();
	}

	setup() {
		this.rootNode = this.settings.rootNode;
		this.setupOptions();
	}

	setupOptions() {
		if (this.options.animationModule) {
			// ...
		}
	}

	traverse() {
		// ...
	}
}

const $ = new DOMTraverser({
	rootNode: document.getElementsByTagName("body"),
	options: {
		animationModule() {},
	},
});
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### مبدأ عكس التابعيّة Dependency Inversion Principle (DIP)

ينص هذا المبدأ على شيئين أساسيين:

1. يجب ألا تعتمد الوحدات عالية المستوى (high-level modules) على وحدات منخفضة المستوى (low-level modules). كلاهما يجب أن يعتمد على التجريدات (abstractions).
2. يجب ألا تعتمد التجريدات (abstractions) على التفاصيل. بل يجب أن تعتمد التفاصيل على التجريدات.

قد يكون من الصعب فهم هذا في البداية، ولكن إذا كنت قد تعاملت مع مكتبة AngularJS، فقد رأيت تطبيقًا لهذا المبدأ في شكل حقن التبعية (Dependency (Injection. على الرغم من أنها ليست مفاهيمًا متطابقة، إلا أن حقن التبعية تمنع الوحدات عالية المستوى من معرفة تفاصيل الوحدات ذات المستوى المنخفض وإعدادها.

يمكنها تحقيق ذلك من خلال حقن التبعية. فائدة كبيرة منها أنها تقلل من الترابط (coupling) بين الوحدات. يعد الترابط نمط تطوير سيئ للغاية؛ لأنه يجعل من الصعب إعادة هيكلة الكود (code refactor).

كما ذكرنا سابقًا، لا تحتوي جافاسكريبت على واجهات (interfaces)، لذا فإن التجريدات التي تعتمد عليها هي عقود (contracts) ضمنية. بمعنى آخر، تعتمد على الدوال (methods) والخصائص (properties) التي يعرضها الكائن أو الصنف لكائن أو صنف آخر.
في المثال أدناه، العقد الضمني هو أن أي وحدة طلب (Request module) لصنف متابعة المخزون "InventoryTracker" سيكون لها دالة عناصر الطلب "requestItems".

**خطأ:**

```javascript
class InventoryRequester {
	constructor() {
		this.REQ_METHODS = ["HTTP"];
	}

	requestItem(item) {
		// ...
	}
}

class InventoryTracker {
	constructor(items) {
		this.items = items;

		// سيء: لقد أنشأنا اعتمادية على تنفيذ طلب معيّن

		// يجب أن يكون لدينا فقط عناصر الطلب "requestItems" تعتمد على دالة الطلب: `request`

		this.requester = new InventoryRequester();
	}

	requestItems() {
		this.items.forEach((item) => {
			this.requester.requestItem(item);
		});
	}
}

const inventoryTracker = new InventoryTracker(["apples", "bananas"]);
inventoryTracker.requestItems();
```

**صواب:**

```javascript
class InventoryTracker {
	constructor(items, requester) {
		this.items = items;
		this.requester = requester;
	}

	requestItems() {
		this.items.forEach((item) => {
			this.requester.requestItem(item);
		});
	}
}

class InventoryRequesterV1 {
	constructor() {
		this.REQ_METHODS = ["HTTP"];
	}

	requestItem(item) {
		// ...
	}
}

class InventoryRequesterV2 {
	constructor() {
		this.REQ_METHODS = ["WS"];
	}

	requestItem(item) {
		// ...
	}
}

// من خلال بناء الاعتماديات خارجيًا وحقنها، يمكننا ذلك بسهولة
// استبدال وحدة الطلب بوحدة جديدة رائعة تستخدم تقنية WebSockets.

const inventoryTracker = new InventoryTracker(
	["apples", "bananas"],
	new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

## **الاختبار**

الاختبار أهم من الإطلاق السريع للبيئة التشغيلية. إذا لم تكن لديك اختبارات أو كان عددها غير كافٍ، ففي كل مرة تقوم فيها بإطلاق الكود لن تكون متأكدًا من أنك لم تفسد أي شيء. إن اتخاذ قرار بشأن العدد المناسب للاختبارات متروك لفريقك، ولكن الحصول على تغطية بنسبة 100٪ (جميع البيانات والفروع) هو كيفية تحقيق ثقة عالية جدًا وراحة بال المطورين. هذا يعني أنه بالإضافة إلى وجود إطار عمل اختبار رائع، فإنك تحتاج أيضًا إلى استخدام [أداة تغطية جيدة](https://gotwarlost.github.io/istanbul/).

ليس هناك أي عذر لعدم كتابة الاختبارات. هناك [الكثير من أطر عمل اختبار جافاسكريبت الجيدة](https://jstherightway.org/#testing-tools)، لذا ابحث عن إطار يفضله فريقك. عندما تجد واحدًا يناسب فريقك، فاهدف دائمًا إلى كتابة اختبارات لكل ميزة/وحدة جديدة تقدّمها. إذا كانت طريقتك المفضلة هي التطوير المقاد بالاختبار (TDD)، فهذا أمر رائع، ولكن النقطة الأساسية هي التأكد من وصولك إلى أهداف التغطية قبل إطلاق أي ميزة، أو إعادة هيكلة ميزة موجودة مسبقًا.

### مفهوم واحد لكل اختبار

**خطأ:**

```javascript
import assert from "assert";

describe("MomentJS", () => {
	it("handles date boundaries", () => {
		let date;

		date = new MomentJS("1/1/2015");
		date.addDays(30);
		assert.equal("1/31/2015", date);

		date = new MomentJS("2/1/2016");
		date.addDays(28);
		assert.equal("02/29/2016", date);

		date = new MomentJS("2/1/2015");
		date.addDays(28);
		assert.equal("03/01/2015", date);
	});
});
```

**صواب:**

```javascript
import assert from "assert";

describe("MomentJS", () => {
	it("handles 30-day months", () => {
		const date = new MomentJS("1/1/2015");
		date.addDays(30);
		assert.equal("1/31/2015", date);
	});

	it("handles leap year", () => {
		const date = new MomentJS("2/1/2016");
		date.addDays(28);
		assert.equal("02/29/2016", date);
	});

	it("handles non-leap year", () => {
		const date = new MomentJS("2/1/2015");
		date.addDays(28);
		assert.equal("03/01/2015", date);
	});
});
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

## **التزامن**

### استخدم الوعود (promises)، وليس دوال رد النداء (callbacks)

دوال رد النداء (callbacks) ليست أكوادًا نظيفة، وتسبب تداخلًا مفرطًا. مع الإصدارات الحديثة ES2015/ES6، تعد الوعود نوعًا عامًا مدمجًا. استخدمها!

**خطأ:**

```javascript
import { get } from "request";
import { writeFile } from "fs";

get(
	"https://en.wikipedia.org/wiki/Robert_Cecil_Martin",
	(requestErr, response, body) => {
		if (requestErr) {
			console.error(requestErr);
		} else {
			writeFile("article.html", body, (writeErr) => {
				if (writeErr) {
					console.error(writeErr);
				} else {
					console.log("File written");
				}
			});
		}
	}
);
```

**صواب:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
	.then((body) => {
		return writeFile("article.html", body);
	})
	.then(() => {
		console.log("File written");
	})
	.catch((err) => {
		console.error(err);
	});
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### استخدام Async/Await أنظف حتى من الوعود (Promises)

تعد الوعود (promises) بديلاً نظيفًا جدًا لدوال رد النداء (callbacks)، ولكن ES2017/ES8 تجلب `async` و`await` والتي تقدّم كودًا أكثر نظافة. كل ما تحتاجه هو دالة مسبوقة في بالكلمة المفتاحية `async`، وبعد ذلك يمكنك كتابة الكود دون دوال `then` المسلسلة. استخدم هذا إذا كان بإمكانك الاستفادة من ميزات ES2017/ES8 اليوم!

**خطأ:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
	.then((body) => {
		return writeFile("article.html", body);
	})
	.then(() => {
		console.log("File written");
	})
	.catch((err) => {
		console.error(err);
	});
```

**صواب:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

async function getCleanCodeArticle() {
	try {
		const body = await get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin");
		await writeFile("article.html", body);
		console.log("File written");
	} catch (err) {
		console.error(err);
	}
}

getCleanCodeArticle();
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

## **التعامل مع الأخطاء**

تعد الأخطاء الملقاة (thrown errors) شيء جيد! إنها تعني أن وقت التشغيل قد حُدّد بنجاح عندما حدث خطأ ما في برنامجك ويخبرك عن طريق إيقاف تنفيذ الوظيفة على المكدّس الحالي (current stack)، وقتل العملية (في Node)، وتنبيهك في وحدة التحكم بتتبع المكدس (stack trace).

### لا تتجاهل الأخطاء المُكتَشفة

عدم القيام بأي شيء مع وجود خطأ مكتشف لا يمنحك القدرة على إصلاح الخطأ أو التعامل معه. لا يعد تسجيل الخطأ في وحدة التحكم عن طريق الأمر (`console.log`) أفضل كثيرًا لأنه في كثير من الأحيان يمكن أن يضيع في بحر من النصوص المطبوعة على وحدة التحكم (console).
إذا أحطت أي جزء من الكود في `try/catch`، فهذا يعني أنك تظن أنه قد يحدث خطأ هناك، وبالتالي يجب أن يكون لديك خطة، أو إنشاء مسار للكود عند حدوث الخطأ.

**خطأ:**

```javascript
try {
	functionThatMightThrow();
} catch (error) {
	console.log(error);
}
```

**صواب:**

```javascript
try {
	functionThatMightThrow();
} catch (error) {
	// الخيار الأول (أكثر ضوضاء من console.log)
	console.error(error);
	// خيار آخر:
	notifyUserOfError(error);
	// خيار آخر:
	reportErrorToService(error);
	// أو عمل الجميع!
}
```

### لا تتجاهل الوعود المرفوضة (rejected promises)

لنفس السبب لا ينبغي أن تتجاهل الأخطاء التي اكتشفت من `try/catch`.

**خطأ:**

```javascript
getdata()
	.then((data) => {
		functionThatMightThrow(data);
	})
	.catch((error) => {
		console.log(error);
	});
```

**صواب:**

```javascript
getdata()
	.then((data) => {
		functionThatMightThrow(data);
	})
	.catch((error) => {
		// الخيار الأول (أكثر ضوضاء من console.log)

		console.error(error);
		// خيار آخر:

		notifyUserOfError(error);
		// خيار آخر:

		reportErrorToService(error);
		// أو عمل الجميع!
	});
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

## **التنسيق**

التنسيق أمر شخصي. مثل العديد من القواعد الواردة هنا، لا توجد قاعدة صارمة وسريعة يجب عليك اتباعها دون وعي. النقطة الأساسية هي عدم الجدال على التنسيق. هناك [الكثير من الأدوات](https://standardjs.com/rules.html) لأتمتة هذا. استخدم واحدة! إن الجدل حول التنسيق ما هو إلا مضيعة للوقت والمال.

بالنسبة للأشياء التي لا تقع ضمن نطاق التنسيق التلقائي: كالمسافة البادئة (indentation)، وعلامات التبويب مقابل المسافات (tabs vs. spaces)، وعلامات الاقتباس المزدوجة مقابل علامات الاقتباس المفردة (double vs. single quotes)، إلخ، فابحث هنا عن بعض الإرشادات.

### استخدام الأحرف الكبيرة باتساق

جافاسكريبت هي لغة غير محددة أنواع البيانات، لذا فإن الكتابة بالأحرف الكبيرة تخبرك كثيرًا عن المتغيرات والدوال وما إلى ذلك. هذه القواعد ذاتية، لذا يمكن لفريقك اختيار ما يريده. النقطة المهمة هي الاتساق بغض النظر عما تختاره أنت وفريقك.

**خطأ:**

```javascript
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const Artists = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restore_database() {}

class animal {}
class Alpaca {}
```

**صواب:**

```javascript
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const SONGS = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const ARTISTS = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restoreDatabase() {}

class Animal {}
class Alpaca {}
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### يجب أن تكون الدوال المنادِية (callers) والمنادى عليها (callees) قريبة من بعضها

إذا استدعت دالة أخرى، احتفظ بهذه الدوال قريبة عموديًا في ملف المصدر. من الناحية المثالية، احتفظ بالدالة المنادية فوق الدالة المنادى عليها مباشرة. نميل إلى قراءة الأكواد من أعلى إلى أسفل مثل الجريدة. لهذا السبب، اجعل الكود يقرأ بهذه الطريقة.

**خطأ:**

```javascript
class PerformanceReview {
	constructor(employee) {
		this.employee = employee;
	}

	lookupPeers() {
		return db.lookup(this.employee, "peers");
	}

	lookupManager() {
		return db.lookup(this.employee, "manager");
	}

	getPeerReviews() {
		const peers = this.lookupPeers();
		// ...
	}

	perfReview() {
		this.getPeerReviews();
		this.getManagerReview();
		this.getSelfReview();
	}

	getManagerReview() {
		const manager = this.lookupManager();
	}

	getSelfReview() {
		// ...
	}
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**صواب:**

```javascript
class PerformanceReview {
	constructor(employee) {
		this.employee = employee;
	}

	perfReview() {
		this.getPeerReviews();
		this.getManagerReview();
		this.getSelfReview();
	}

	getPeerReviews() {
		const peers = this.lookupPeers();
		// ...
	}

	lookupPeers() {
		return db.lookup(this.employee, "peers");
	}

	getManagerReview() {
		const manager = this.lookupManager();
	}

	lookupManager() {
		return db.lookup(this.employee, "manager");
	}

	getSelfReview() {
		// ...
	}
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

## **التعليقات**

### اكتب التعليقات فقط على الأشياء التي بها منطق عمل معقد (business logic complexity).

ليست التعليقات ضرورة مُلحّة. الكود الجيد _ تقريبًا_ يوثّق نفسه.

**خطأ:**

```javascript
function hashIt(data) {
	// التجزئة
	let hash = 0;

	// طول النص
	const length = data.length;

	// الدوران على كل حرف في البيانات
	for (let i = 0; i < length; i++) {
		// الحصول على كود الحرف
		const char = data.charCodeAt(i);
		// إنشاء التجزئة
		hash = (hash << 5) - hash + char;
		// التحويل إلى عدد صحيح من 32 بت
		hash &= hash;
	}
}
```

**صواب:**

```javascript
function hashIt(data) {
	let hash = 0;
	const length = data.length;

	for (let i = 0; i < length; i++) {
		const char = data.charCodeAt(i);
		hash = (hash << 5) - hash + char;

		// التحويل إلى عدد صحيح من 32 بت
		hash &= hash;
	}
}
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### لا تترك الأكواد المعلّقة في الكود

أضيفت برمجيات التحكم في الإصدار (version control) لسبب وجيه. اترك الكود القديم في سجلّك.

**خطأ:**

```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**صواب:**

```javascript
doStuff();
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### لا تحتفظ بالتعليقات التي تشبه المذكرات اليومية

تذكر، استخدم برمجيات التحكم في الإصدار! ليست هناك حاجة للكود الميت، ولا الأكواد المعلّقة، وخاصة تعليقات المذكرات اليومية. استخدم `git log` للحصول على السجل!

**خطأ:**

```javascript
/**
 * 2016-12-20: حذف الmonads، لم أفهمها (مستخدم أ)
 * 2016-10-01: تحسين استخدام نوع خاص من monads (مستخدم ب)
 * 2016-02-03: حذف التحقق من أنواع البيانات (مستخدم ج)
 */
function combine(a, b) {
	return a + b;
}
```

**صواب:**

```javascript
function combine(a, b) {
	return a + b;
}
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

### تجنب العلامات الموضعية

عادة ما يضفن ضوضاء لا أكثر. دع الدوال وأسماء المتغيرات جنبًا إلى جنب مع المسافة البادئة المناسبة وسيعطي التنسيق البنية المرئية للكود.

**خطأ:**
```javascript
////////////////////////////////////////////////////////////////////////////////
// إنشاء نموذج Scope
////////////////////////////////////////////////////////////////////////////////
$scope.model = {
	menu: "foo",
	nav: "bar",
};

////////////////////////////////////////////////////////////////////////////////
// إعداد ال action
////////////////////////////////////////////////////////////////////////////////
const actions = function () {
	// ...
};
```

**صواب:**

```javascript
$scope.model = {
	menu: "foo",
	nav: "bar",
};

const actions = function () {
	// ...
};
```

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

## الترجمة

هذا الدليل متوفر أيضًا بلغات أخرى:

- ![am](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/flat/24/Armenia.png) **Armenian**: [hanumanum/clean-code-javascript/](https://github.com/hanumanum/clean-code-javascript)
- ![ar](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Saudi-Arabia.png) **Arabic - العربية**: [imAbdelhadi/clean-code-javascript-in-arabic](https://github.com/imAbdelhadi/clean-code-javascript-in-arabic/)
- ![bd](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bangladesh.png) **Bangla(বাংলা)**: [InsomniacSabbir/clean-code-javascript/](https://github.com/InsomniacSabbir/clean-code-javascript/)
- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [fesnt/clean-code-javascript](https://github.com/fesnt/clean-code-javascript)
- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Simplified Chinese**:
  - [alivebao/clean-code-js](https://github.com/alivebao/clean-code-js)
  - [beginor/clean-code-javascript](https://github.com/beginor/clean-code-javascript)
- ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Traditional Chinese**: [AllJointTW/clean-code-javascript](https://github.com/AllJointTW/clean-code-javascript)
- ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [eugene-augier/clean-code-javascript-fr](https://github.com/eugene-augier/clean-code-javascript-fr)
- ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [marcbruederlin/clean-code-javascript](https://github.com/marcbruederlin/clean-code-javascript)
- ![id](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Indonesia.png) **Indonesia**: [andirkh/clean-code-javascript/](https://github.com/andirkh/clean-code-javascript/)
- ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**: [frappacchio/clean-code-javascript/](https://github.com/frappacchio/clean-code-javascript/)
- ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/clean-code-javascript/](https://github.com/mitsuruog/clean-code-javascript/)
- ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [qkraudghgh/clean-code-javascript-ko](https://github.com/qkraudghgh/clean-code-javascript-ko)
- ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [greg-dev/clean-code-javascript-pl](https://github.com/greg-dev/clean-code-javascript-pl)
- ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**:
  - [BoryaMogila/clean-code-javascript-ru/](https://github.com/BoryaMogila/clean-code-javascript-ru/)
  - [maksugr/clean-code-javascript](https://github.com/maksugr/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [tureey/clean-code-javascript](https://github.com/tureey/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Uruguay.png) **Spanish**: [andersontr15/clean-code-javascript](https://github.com/andersontr15/clean-code-javascript-es)
- ![rs](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Serbia.png) **Serbian**: [doskovicmilos/clean-code-javascript/](https://github.com/doskovicmilos/clean-code-javascript)
- ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **Turkish**: [bsonmez/clean-code-javascript](https://github.com/bsonmez/clean-code-javascript/tree/turkish-translation)
- ![ua](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Ukraine.png) **Ukrainian**: [mindfr1k/clean-code-javascript-ua](https://github.com/mindfr1k/clean-code-javascript-ua)
- ![vi](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamese**: [hienvd/clean-code-javascript/](https://github.com/hienvd/clean-code-javascript/)
- ![ir](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Iran.png) **Persian**: [hamettio/clean-code-javascript](https://github.com/hamettio/clean-code-javascript)

**[⬆ العودة إلى الأعلى](#جدول-المحتويات)**

ترجمه إلى العربية واثق الشويطر وعبدالهادي الأندلسي -وبتصرف- لدليل [clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript)

شكر وامتنان لكل من راجع الترجمة (القائمة أبجدية):
- [عبد العزيز مخناش](https://twitter.com/Abdelaziz18003)
- [عيسى بوكرن](https://twitter.com/tutomena)
- [معتز الخطيب](https://twitter.com/muotaz5)
- [ناصر داخل](https://www.linkedin.com/in/naser-dakhel)

