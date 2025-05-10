# Android Compose Mini Projects

این مخزن شامل دو پروژه کوچک و ساده با استفاده از Kotlin و Jetpack Compose در اندروید است. این تمرین‌ها برای دانشجویان مبتدی طراحی شده‌اند تا با مفاهیم پایه Compose و مدیریت حالت (State) آشنا شوند.

## 🚀 تمرین اول: ماشین حساب ساده

این تمرین شامل یک ماشین حساب ساده است که می‌تواند عملیات اصلی ریاضی (جمع، تفریق، ضرب، و تقسیم) را انجام دهد.

### ✨ ویژگی‌ها:

* دو TextField برای ورود عدد اول و عدد دوم.
* چهار دکمه برای انجام عملیات ریاضی.
* نمایش نتیجه محاسبه در پایین.

### 📌 نحوه عملکرد:

* دانشجو عدد اول و دوم را وارد می‌کند.
* با کلیک بر روی هر دکمه (مثل +، -، ×، ÷)، عملیات مربوطه انجام شده و نتیجه نمایش داده می‌شود.
* در صورت تقسیم بر صفر، پیام "تقسیم بر صفر مجاز نیست!" نمایش داده می‌شود.

### ✅ پاسخ تمرین (کد):

```kotlin
@Composable
fun SimpleCalculator() {
    var number1 by remember { mutableStateOf("") }
    var number2 by remember { mutableStateOf("") }
    var result by remember { mutableStateOf("") }

    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Text(
            text = "ماشین حساب ساده",
            fontSize = 24.sp,
            fontWeight = FontWeight.Bold,
            color = Color(0xFF00796B)
        )

        Spacer(modifier = Modifier.height(16.dp))

        TextField(
            value = number1,
            onValueChange = {
                if (it.all { char -> char.isDigit() || char == '.' }) {
                    number1 = it
                }
            },
            label = { Text("عدد اول") },
            keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Number),
        )

        Spacer(modifier = Modifier.height(8.dp))

        TextField(
            value = number2,
            onValueChange = {
                if (it.all { char -> char.isDigit() || char == '.' }) {
                    number2 = it
                }
            },
            label = { Text("عدد دوم") },
            keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Number),
        )

        Spacer(modifier = Modifier.height(16.dp))

        Row(
            horizontalArrangement = Arrangement.SpaceEvenly,
            modifier = Modifier.fillMaxWidth()
        ) {
            Button(
                onClick = { result = calculateResult(number1, number2, "+") },
                modifier = Modifier
                    .padding(4.dp)
                    .weight(1f)
            ) {
                Text(text = "+", fontSize = 20.sp, fontWeight = FontWeight.Bold)
            }

            Button(
                onClick = { result = calculateResult(number1, number2, "-") },
                modifier = Modifier
                    .padding(4.dp)
                    .weight(1f)
            ) {
                Text(text = "-", fontSize = 20.sp, fontWeight = FontWeight.Bold)
            }

            Button(
                onClick = { result = calculateResult(number1, number2, "*") },
                modifier = Modifier
                    .padding(4.dp)
                    .weight(1f)
            ) {
                Text(text = "×", fontSize = 20.sp, fontWeight = FontWeight.Bold)
            }

            Button(
                onClick = { result = calculateResult(number1, number2, "/") },
                modifier = Modifier
                    .padding(4.dp)
                    .weight(1f)
            ) {
                Text(text = "÷", fontSize = 20.sp, fontWeight = FontWeight.Bold)
            }
        }

        Spacer(modifier = Modifier.height(16.dp))

        Text(
            text = "نتیجه: $result",
            fontSize = 20.sp,
            fontWeight = FontWeight.Medium,
            color = Color(0xFF004D40)
        )
    }
}

fun calculateResult(num1: String, num2: String, operator: String): String {
    val n1 = num1.toDouble()
    val n2 = num2.toDouble()
    var result = ""
    if (operator == "+") result = (n1 + n2).toString()
    else if (operator == "-") result = (n1 - n2).toString()
    else if (operator == "*") result = (n1 * n2).toString()
    else if (operator == "/") {
        if (n2 != 0.0) result = (n1 / n2).toString() else result = "تقسیم بر صفر مجاز نیست!"
    }
    return result
}

```

---

## 🚀 تمرین دوم: بازی حدس عدد

این تمرین شامل یک بازی ساده حدس عدد است که در آن کاربر باید عدد مخفی بین 1 تا 100 را حدس بزند.

### ✨ ویژگی‌ها:

* کاربر عددی را وارد می‌کند و حدس می‌زند.
* برنامه بررسی می‌کند که عدد وارد شده بزرگ‌تر، کوچک‌تر یا مساوی عدد مخفی است.
* لیستی از حدس‌های کاربر در پایین نمایش داده می‌شود.
* عدد مخفی هر بار به صورت تصادفی تولید می‌شود.

### 📌 نحوه عملکرد:

* دانشجو یک عدد وارد می‌کند و روی "ثبت حدس" کلیک می‌کند.
* برنامه به کاربر می‌گوید که عدد بزرگ‌تر یا کوچک‌تر است یا صحیح است.
* حدس‌های کاربر در لیست پایین نمایش داده می‌شوند.

### ✅ پاسخ تمرین (کد):

```kotlin
@Composable
fun NumberGuessGame() {
    var input by remember { mutableStateOf("") }
    val guesses = remember { mutableStateListOf<String>() }
    val secretNumber = remember { Random.nextInt(1, 101) }
    var message by remember { mutableStateOf("") }

    Column(modifier = Modifier.padding(16.dp)) {
        TextField(
            value = input,
            onValueChange = { input = it },
            label = { Text("یک عدد حدس بزن") },
            modifier = Modifier.fillMaxWidth()
        )

        Spacer(modifier = Modifier.height(8.dp))

        Button(onClick = {
            val guess = input.toInt()
            guesses.add("حدس: $guess")
            message = if (guess < secretNumber) {
                "عدد بزرگ‌تره"
            } else if (guess > secretNumber) {
                "عدد کوچک‌تره"
            } else {
                "درست گفتی!"
            }
            input = ""
        }) {
            Text("ثبت حدس")
        }

        Spacer(modifier = Modifier.height(8.dp))
        Text(text = message, fontSize = 18.sp)

        LazyColumn {
            items(guesses) { guessText ->
                Text(
                    text = guessText,
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(4.dp)
                )
            }
        }
    }
}


```
