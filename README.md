---
description: 取得帶有標記 (tag) 的指標 (pointer) 資料，簡稱：TBP
---

# WinShop 取得指標與標記所屬內容資料幫助函式

### 函式起始

使用 `TBP()` 開頭，後面可以使用 `->` 串接附帶函式增加條件

* 第一參數為必填，請輸入指標名稱
* 第二參數為選填，請輸入布林代數
  * 如果為 "是"，第一參數改為輸入資料 url 查詢
  * 如果為 "否" 或是不輸入，則是原本以指標進行搜尋功能

{% tabs %}
{% tab title="範例 1" %}
**假設有一指標 `abc`。**

```php
TBP('abc');
```
{% endtab %}

{% tab title="範例 2" %}
**假設有一指標以變數帶入。**

```php
TBP($pointer);
```
{% endtab %}

{% tab title="範例 3" %}
**假設以商品首頁 url 搜尋。**

```php
TBP('products', true);
```
{% endtab %}
{% endtabs %}

***

### 函式 `find()`

指定查詢條件，可以輸入一項參數，以下是可以使用的各參數及其意義：

<table><thead><tr><th width="141.73828125">參數</th><th width="206.53125">意義說明</th><th>範例</th></tr></thead><tbody><tr><td><code>children</code></td><td>搜尋指定指標子層資料</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->find('children');
</code></pre></td></tr><tr><td><code>descendants</code></td><td>搜尋指定指標下層資料</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->find('descendants');
</code></pre></td></tr><tr><td><code>parent</code></td><td>搜尋指定指標父層資料</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->find('parent');
</code></pre></td></tr><tr><td><code>ancestors</code></td><td>搜尋指定指標上層資料</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->find('ancestors');
</code></pre></td></tr><tr><td><code>leaf</code></td><td>搜尋指定指標底層資料</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->find('leaf');
</code></pre></td></tr><tr><td><code>self</code></td><td>搜尋指定指標資料</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->find('self');
</code></pre></td></tr></tbody></table>

{% hint style="info" %}
如果沒有使用此函式，`TBP()` 一律以 `children` 條件查詢。
{% endhint %}

***

### 函式 `tag()`

指定查詢的標記，可以輸入一項參數，以下是可以使用的各參數及其意義：

<table><thead><tr><th width="141.73828125">參數</th><th width="206.53125">意義說明</th><th>範例</th></tr></thead><tbody><tr><td><code>new</code></td><td>最新</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->tag('new');
</code></pre></td></tr><tr><td><code>hot</code></td><td>熱門</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->tag('hot');
</code></pre></td></tr><tr><td><code>sale</code></td><td>促銷</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->tag('sale');
</code></pre></td></tr><tr><td><code>custom1</code></td><td>自訂 1</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->tag('custom1');
</code></pre></td></tr><tr><td><code>custom2</code></td><td>自訂 2</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->tag('custom2');
</code></pre></td></tr></tbody></table>

{% hint style="info" %}
如果沒有使用，則不限制查詢資料
{% endhint %}

#### 函式可以多個串聯，範例：

```php
TBP($pointer)->tag('new')->tag('sale')->get();
```

***

### 函式 `sort()`

指定查詢的排序，可以輸入一項參數，以下是可以使用的各參數及其意義：

<table><thead><tr><th width="141.73828125">參數</th><th width="253.5078125">意義說明</th><th>範例</th></tr></thead><tbody><tr><td><code>nested</code></td><td>依照分類排序</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->sort('nested');
</code></pre></td></tr><tr><td><code>create</code></td><td>依照建立時間排序</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->sort('create');
</code></pre></td></tr><tr><td><code>create_desc</code></td><td>依照建立時間反向排序</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->sort('create_desc');
</code></pre></td></tr><tr><td><code>public</code></td><td>依照顯示時間排序、如果沒有顯示時間 <code>public_at</code> 則以建立時間排序</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->sort('public');
</code></pre></td></tr><tr><td><code>public_desc</code></td><td>依照顯示時間反向排序、如果沒有顯示時間 <code>public_at</code> 則以建立時間反向排序</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->sort('public_desc');
</code></pre></td></tr><tr><td><code>enable</code></td><td>依照啟用時間排序、如果沒有啟用時間 <code>enable_at</code> 則以建立時間排序</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->sort('enable');
</code></pre></td></tr><tr><td><code>enable_desc</code></td><td>依照啟用時間反向排序、如果沒有啟用時間 <code>enable_at</code> 則以建立時間反向排序</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->sort('enable_desc');
</code></pre></td></tr></tbody></table>

{% hint style="info" %}
如果沒有使用，一律以 `nested` 條件排序。
{% endhint %}

***

### 函式 `limit()`

指定查詢的數量，如果有使用必填輸入一項整數參數。

#### 範例

```php
TBP($pointer)->limit(1); // 限制查詢一筆資料
TBP($pointer)->limit(2); // 限制查詢二筆資料
TBP($pointer)->limit(30); // 限制查詢三十筆資料
```

{% hint style="info" %}
如果沒有使用，沒有限制數量。
{% endhint %}

{% hint style="warning" %}
請注意！如果是查詢多個模組，每個模組會各自計算查詢數量。
{% endhint %}

***

### 函式 `populate()`

指定查詢資料的關聯資料，可以輸入一項陣列參數，以下是可以使用的各陣列元素及其意義：

<table><thead><tr><th width="159.89453125">參數</th><th width="144.34765625">意義說明</th><th>範例</th></tr></thead><tbody><tr><td>nested</td><td>分類樹節點資料</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->populate(['nested']);
</code></pre></td></tr><tr><td>images</td><td>圖片資料</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->populate(['images']);
</code></pre></td></tr><tr><td>attach</td><td>附件資料</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->populate(['attach']);
</code></pre></td></tr><tr><td>seo</td><td>SEO 資料關聯</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->populate(['seo']);
</code></pre></td></tr><tr><td>tag</td><td>標記關聯</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->populate(['tag']);
</code></pre></td></tr><tr><td>state</td><td>啟用狀態關聯</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->populate(['state']);
</code></pre></td></tr><tr><td>stock</td><td>庫存關聯</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->populate(['stock']);
</code></pre></td></tr><tr><td>ship_exclude</td><td>排除的運費規格</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->populate(['ship_exclude']);
</code></pre></td></tr><tr><td>ship_scale_match</td><td>運費規格關聯</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->populate(['ship_scale_match']);
</code></pre></td></tr><tr><td>description</td><td>描述資料</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->populate(['description']);
</code></pre></td></tr><tr><td>redirect</td><td>重導向</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->populate(['redirect']);
</code></pre></td></tr><tr><td>finance_clone</td><td>關聯的投資人專區</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->populate(['finance_clone']);
</code></pre></td></tr><tr><td>lockedModel</td><td>鎖定模式</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->populate(['lockedModel']);
</code></pre></td></tr></tbody></table>

{% hint style="info" %}
如果沒有使用，一律以 `['images', 'description']` 作為關聯條件。
{% endhint %}

***

### 函式 `get()`

結尾函式，最後串連呼叫的函式，才能取得查詢的資料。

{% hint style="info" %}
除了結尾函式與起始函式 `TBP()` 以外，其他函式沒有順序分別。
{% endhint %}

可以輸入一項參數，以下是可以使用的各參數及其意義：

<table><thead><tr><th width="112.18359375">參數</th><th width="299.125">意義說明</th><th>範例</th></tr></thead><tbody><tr><td><code>products</code></td><td>僅取出商品資料</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->get('products');
</code></pre></td></tr><tr><td><code>news</code></td><td>僅取出最新消息資料</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->get('news');
</code></pre></td></tr><tr><td><code>message</code></td><td>僅取出訊息管理資料</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->get('message');
</code></pre></td></tr><tr><td><code>aboutus</code></td><td>僅取出關於我們資料</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->get('aboutus');
</code></pre></td></tr><tr><td><code>download</code></td><td>僅取出下載檔案資料</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->get('download');
</code></pre></td></tr><tr><td><code>qa</code></td><td>僅取出常見問題資料</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->get('qa');
</code></pre></td></tr><tr><td><code>null</code></td><td>取出所有模組資料<br>(<code>null</code> 不是字串，不要用單引號包裹)</td><td><pre class="language-php"><code class="lang-php">TBP($pointer)->get(null);
</code></pre></td></tr></tbody></table>

{% hint style="info" %}
如果沒有填寫參數，預設查詢訊息管理資料：`message`。
{% endhint %}

{% tabs %}
{% tab title="範例 1" %}
只設定指標 `$pointer`，其他都預設。

```php
TBP($pointer)->get();
```
{% endtab %}

{% tab title="範例 2" %}
指標 `abc`、查詢所有最下層 (內容) 資料、以發佈日期反向排序、\
帶有最新標記、僅取得最新消息資料。

```php
TBP('abc')->find('leaf')->tag('new')->sort('public_desc')->get('news');
```
{% endtab %}

{% tab title="範例 3" %}
指標 `$pointer`、查詢所有上層資料、僅取得商品資料。

```php
TBP($pointer)->find('ancestors')->get('products');
```
{% endtab %}

{% tab title="範例 4" %}
以某商品資料 url、查詢所有上層資料。

```php
TBP('water', true)->find('ancestors')->get();
```
{% endtab %}

{% tab title="範例 5" %}
跨模組查詢同指標資料。

```php
TBP($pointer)->find('leaf')->get(null);
```
{% endtab %}
{% endtabs %}

***

### 函式 `paginate()`

分頁結尾函式，最後串連呼叫的函式，才能取得查詢的資料。

{% hint style="info" %}
除了結尾函式與起始函式 `TBP()` 以外，其他函式沒有順序分別。
{% endhint %}

#### 此函式有三個參數：

* 第一項參數與函式 `get()` 相同。
* 第二項參數為正整數，代表每頁資料筆數。
* 第三項參數為正整數，代表分頁號碼
  * 如果沒有指定會自動抓取網頁 `page` 網址參數。
  * 完全沒有參數預設是第 1 頁。

{% tabs %}
{% tab title="範例 1" %}
只設定指標 `$pointer`，其他都預設。

```php
TBP($pointer)->paginate();
```
{% endtab %}

{% tab title="範例 2" %}
指標 `abc`、查詢所有最下層 (內容) 資料、以發佈日期反向排序、\
帶有最新標記、僅取得最新消息資料。

```php
TBP('abc')->find('leaf')->tag('new')->sort('public_desc')->paginate('news');
```
{% endtab %}

{% tab title="範例 3" %}
雷同範例 2，增加限制輸出 20 筆資料限制。

```php
TBP('abc')->find('leaf')->tag('new')->sort('public_desc')->paginate('news', 20);
```
{% endtab %}

{% tab title="範例 4" %}
雷同範例 3 增加輸出第 2 頁資料。

```php
TBP('abc')->find('leaf')->tag('new')->sort('public_desc')->paginate('news', 20, 2);
```
{% endtab %}
{% endtabs %}

***

### 函式 `testMode()`

結尾函式，測試模式輸出，使用方式同函式 `get()`。

{% hint style="info" %}
除了結尾函式與起始函式 `TBP()` 以外，其他函式沒有順序分別。
{% endhint %}

輸出查詢資料用的 SQL 語句集合而不是資料。

#### 範例

#### 只設定指標 `$pointer`，其他都預設，輸出 SQL 語法集合。

```php
TBP($pointer)->testMode();
```
