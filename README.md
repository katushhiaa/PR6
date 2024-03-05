# Робота із віджетами

**Тема роботи:** Робота із віджетами.

**Мета роботи:** Ознайомитись з використанням віджетів TabWidget, WebView, ListView.

### Завдання 1 «Робота із вкладками»

1. Опис потрібних рядків у файлі  strings.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">TabWidgetSample</string>
    <string name="tab1_indicator">Students</string>
    <string name="tab2_indicator">Teachers</string>
    <string name="tab3_indicator">Classes</string>
    <string name="tab1_content">This is Students tab</string>
    <string name="tab2_content">This is Teachers tab</string>
    <string name="tab3_content">This is Classes tab</string>
</resources>
   ```

Далі порібно було спробувати локалізацію, тому було створено values-fr.xml тільки для французької
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">TabWidgetSample</string>
    <string name="tab1_indicator">Étudiants</string>
    <string name="tab2_indicator">Enseignants</string>
    <string name="tab3_indicator">Classes</string>
    <string name="tab1_content">Ceci est longlet Étudiants</string>
    <string name="tab2_content">Ceci est longlet Enseignants</string>
    <string name="tab3_content">Ceci est longlet Classes</string>
</resources>
```

2. Редагування файлу activity_main.xml для відображення Tabs

```xml
<?xml version="1.0" encoding="utf-8"?>
<TabHost xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@android:id/tabhost"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <LinearLayout
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:padding="5dp">
        <TabWidget
            android:id="@android:id/tabs"
            android:layout_width="fill_parent"
            android:layout_height="wrap_content" />
        <FrameLayout
            android:id="@android:id/tabcontent"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent"
            android:padding="5dp" />
    </LinearLayout>
</TabHost>

```
3. Створення нових активностей StudentsActivity, TeachersActivity і ClassesActivity для відображення вмісту вкладок.

**StudentsActivity**
```java
package com.example.tabwidgetsample;
import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.content.res.Resources;
import android.widget.TextView;

public class StudentsActivity extends AppCompatActivity {
    @Override
    protected	void	onCreate(Bundle	savedInstanceState)	{
        super.onCreate(savedInstanceState);
        Resources res = getResources();
        String contentText = res.getString(R.string.tab1_content);
        TextView textView = new TextView(this);
        textView.setText(contentText); setContentView(textView); }
}
```

**TeachersActivity**
```java
package com.example.tabwidgetsample;
import android.content.res.Resources;
import android.os.Bundle;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;

public class TeachersActivity  extends AppCompatActivity {
    @Override
    protected	void	onCreate(Bundle savedInstanceState)	{
        super.onCreate(savedInstanceState);
        Resources res = getResources();
        String contentText = res.getString(R.string.tab2_content);
        TextView textView = new TextView(this);
        textView.setText(contentText); setContentView(textView); }
}

```
**ClassesActivity**
```java
package com.example.tabwidgetsample;

import android.content.res.Resources;
import android.os.Bundle;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;

public class ClassesActivity extends AppCompatActivity {
    @Override
    protected	void	onCreate(Bundle savedInstanceState)	{
        super.onCreate(savedInstanceState);
        Resources res = getResources();
        String contentText = res.getString(R.string.tab3_content);
        TextView textView = new TextView(this);
        textView.setText(contentText); setContentView(textView); }
}
```

4. Замінити базовий клас для TabWidgetSampleActivity зз Activity на TabActivity і перевизначити метод onCreate

В ньому буде створено самі вкладки, взято їх назви з strings.xml та вміст з активностей StudentsActivity, TeachersActivity і ClassesActivity
   

```java
package com.example.tabwidgetsample;

import android.app.TabActivity;
import android.content.Intent;
import android.content.res.Resources;
import android.os.Bundle;
import android.widget.TabHost;


public class TabWidgetSampleActivity extends TabActivity  {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Resources res = getResources();
        String tab1Indicator = res.getString(R.string.tab1_indicator);
        String tab2Indicator = res.getString(R.string.tab2_indicator);
        String tab3Indicator = res.getString(R.string.tab3_indicator);
        TabHost tabHost = getTabHost();
        TabHost.TabSpec spec;
        Intent intent;
        intent = new Intent().setClass(this, StudentsActivity.class);
        spec = tabHost.newTabSpec("students").setIndicator(tab1Indicator).setContent(intent);
        tabHost.addTab(spec);

        intent = new Intent().setClass(this, TeachersActivity.class);
        spec = tabHost.newTabSpec("teachers").setIndicator(tab2Indicator).setContent(intent);
        tabHost.addTab(spec);

        intent = new Intent().setClass(this, ClassesActivity.class);
        spec = tabHost.newTabSpec("class").setIndicator(tab3Indicator).setContent(intent);
        tabHost.addTab(spec);
        tabHost.setCurrentTab(1);
    }
}
```
 5.   Відредагувати файл AndroidManifest.xml так, щоб всі використовувані активності були в ньому описані.
``` xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools" >

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.Design.NoActionBar"
        tools:targetApi="31">
        <activity android:name=".TabWidgetSampleActivity"
            android:label="@string/app_name"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity android:name=".StudentsActivity" />
        <activity android:name=".TeachersActivity" />
        <activity android:name=".ClassesActivity" />
    </application>

</manifest>
```

Результат виконання програми

https://github.com/katushhiaa/PR6/assets/113555695/435d4f10-8230-4d14-b7b3-1b86f19b298d

### Завдання 2 «Використання WebView»
1. Відредагувати файл activity_main.xml
```xml
  <?xml version="1.0" encoding="utf-8"?>

<WebView
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/webview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" />
```
2. Додавання наступних рядків в метод onCreate, яка створює об'єкт WebView і активує підтримку JavaScript, а також заванатажує сторінку за вказаною веб-адресою

```java
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        webView = findViewById(R.id.webview); // Initializing webView
        webView.setWebViewClient(new MyWebViewClient());
        webView.getSettings().setJavaScriptEnabled(true);
        webView.loadUrl("https://www.google.com.ua");
    }
```
3. Додавання повноваженя на доступ в мережу у manifest файл
```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">
    <uses-permission android:name="android.permission.INTERNET"/>

---other code
</manifest>
```
4. Зміення теми, тим самим дозволивши прибрати показ заголовку. Зробити це можна в файлі manifest
```xml
android:theme="@style/Theme.Design.NoActionBar"
```

5. Додавання до mainfest потрібний фільтр намірів для того обслуговувалось нашим веб-браузером а не стандартним
```xml
<intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
</intent-filter>
```
 6. Винести об'єкт webView з методу onCreate та зробити його членом класу
```java
public class WebViewSampleActivity extends AppCompatActivity {

private WebView webView;
}
```
Створити всередині активності новий клас WebViewSampleClient, що розширює клас WebViewClient.
```java
private static class WebViewSampleClient extends WebViewClient {
    @Override
    public boolean shouldOverrideUrlLoading(WebView view, String url) {
        view.loadUrl(url);
        return true;
    }
}
```
Встановимо свій обробник запитів на відображення веб-ресурсів
```java
webView.setWebViewClient(new WebViewSampleClient());
```

7. Перевизначити метод onKeyDown в активності
```java
    @Override
    public boolean onKeyDown(int keyCode, KeyEvent event) {
        if ((keyCode == KeyEvent.KEYCODE_BACK) && webView.canGoBack()) {
            webView.goBack();
            return true;
        }
        return super.onKeyDown(keyCode, event);
    }
```

8. Результат виконання програми

https://github.com/katushhiaa/PR6/assets/113555695/57b3b975-56d6-4553-86f3-37a5d6d62d26

### Завдання 3 «Використання ListView»
1. У values створити файл arrays.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string-array name="stations">
        <item>Академмістчко</item>
        <item>Житомирська</item>
        <item>Святошин</item>
        <item>Нивки </item>
        <item>Берестейська</item>
        <item>Шулявська</item>
        <item>Політехнічний інститут </item>
        <item>Вокзальна</item>
        <item>Університет</item>
        <item>Татральна</item>
        <item>Хрещатик </item>
        <item>Арсенальна </item>
    </string-array>
</resources>
```
2. Створити файл list_item.xml з наступним вмістом
```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView	xmlns:android="http://schemas.android.com/apk/res/android" 
    android:layout_width="fill_parent" 
    android:layout_height="fill_parent"
    android:padding="10dp" android:textSize="16sp" >
</TextView>

```
3. Модифікувати метод onCreate та додати оброник подій в якості якого буде використовуватися анонімний об'єкт класу OnItemClickListener.
   
```java
package com.example.listviewsample;

import android.app.ListActivity;
import android.content.res.Resources;
import android.os.Bundle;
import android.view.View;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.ListView;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends ListActivity {

    @Override
    public	void	onCreate(Bundle	savedInstanceState)	{
        super.onCreate(savedInstanceState);
        Resources r = getResources();
        String[]	stationsArray	=	r.getStringArray(R.array.stations);
        ArrayAdapter<String> aa = new ArrayAdapter<String>(this, R.layout.list_item, stationsArray);
        setListAdapter(aa);
        ListView lv = getListView();
        lv.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            public void onItemClick(AdapterView<?> parent, View v, int position, long id) {
                CharSequence text = ((TextView) v).getText();
                int duration = Toast.LENGTH_LONG;
                Toast.makeText(getApplicationContext(), text, duration).show();
            }
        });
    }
}
```

4. Результат виконання програми

https://github.com/katushhiaa/PR6/assets/113555695/b0dab43d-2015-488d-b6bd-97243870890d

