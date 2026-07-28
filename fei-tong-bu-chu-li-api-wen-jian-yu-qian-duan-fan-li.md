# 非同步處理 API 文件與前端範例

### API 查詢端點

<table><thead><tr><th width="90.4375">方法</th><th width="151.1328125">路徑</th><th width="253.33203125">Controller</th><th>Service</th></tr></thead><tbody><tr><td>GET</td><td><code>/api/data/tbp</code></td><td><code>FrontController@asyncTagBasedPointer</code></td><td><code>FrontService@asyncTagBasedPointer</code></td></tr></tbody></table>

路由定義於 `routes/api.php`（`data` 前綴群組內），無需驗證。

#### 查詢參數

<table><thead><tr><th width="117.44921875">參數</th><th width="124.02734375">型態</th><th width="87.28515625">必填</th><th>說明</th></tr></thead><tbody><tr><td><code>pointer</code></td><td>string</td><td>是</td><td>指標名稱；<code>is_url</code> 為 <code>1</code> 時改為傳入資料 url</td></tr><tr><td><code>is_url</code></td><td>boolean</td><td>否</td><td>是否以 url 查詢取代指標查詢，預設 <code>false</code></td></tr><tr><td><code>find</code></td><td>string</td><td>否</td><td>查詢類型，預設 <code>children</code>。可用值：<code>children</code>、<code>children_cate</code>（子層且自身有是分類）、<code>descendants</code>、<code>parent</code>、<code>ancestors</code>、<code>siblings</code>、<code>before</code>、<code>after</code>、<code>leaf</code>、<code>self</code></td></tr><tr><td><code>tag</code></td><td>string</td><td>否</td><td>標記篩選，可用值：<code>new</code>、<code>hot</code>、<code>sale</code>、<code>custom1</code>、<code>custom2</code></td></tr><tr><td><code>sort</code></td><td>string</td><td>否</td><td>排序方式，預設 <code>nested</code>。可用值：<code>nested</code>、<code>create</code>、<code>create_desc</code>、<code>public</code>、<code>public_desc</code>、<code>enable</code>、<code>enable_desc</code></td></tr><tr><td><code>limit</code></td><td>integer</td><td>否</td><td>每頁筆數，預設 15</td></tr><tr><td><code>populate</code></td><td>string[]</td><td>否</td><td>附帶關聯資料，如 <code>images</code>、<code>description</code>、<code>nested</code>、<code>tag</code>、<code>state</code>、<code>stock</code> 等</td></tr><tr><td><code>get</code></td><td>string</td><td>否</td><td>輸出模組，預設 <code>message</code>。可用值：<code>products</code>、<code>news</code>、<code>message</code>、<code>aboutus</code>、<code>download</code>、<code>qa</code></td></tr><tr><td><code>page</code></td><td>integer</td><td>否</td><td>分頁頁碼，預設 1；此欄位由  <code>TagBasedPointer::paginate()</code> 內部以 <code>request()->page</code> 讀取，未列於 Controller 的驗證規則中</td></tr></tbody></table>

可用值來源於 `TagBasedPointer::getProperty()`（透過反射取得 `searchTypes` / `tags` / `sortTypes` 屬性預設值），確保 API 驗證規則與 PHP 類別內部允許值同步。

#### 回應格式

回應為 Laravel `LengthAwarePaginator` 序列化結果，包在 `success` 欄位內（見 `App\Traits\ResponseTrait::response()`）：

```json
{
  "success": {
    "current_page": 1,
    "data": [ { "id": 1, "title": "...", "origin_url": "..." } ],
    "last_page": 3,
    "per_page": 15,
    "total": 42
  }
}
```

***

### `TBP` JS 類別

`resources/assets/js/frontend/tbp.js`，用於在瀏覽器端組裝查詢參數並呼叫 `/api/data/tbp`，鏈式 API 對應後端 `TagBasedPointer`：

```js
import TBP from '../../frontend/tbp'

// async await 寫法
const result = await new TBP('products', false)
  .find('leaf')
  .tag('hot')
  .sort('public_desc')
  .populate(['images'])
  .paginate('products', 15, 1)
// result: { data, current_page, last_page, per_page, total, ... }

// then catch 寫法
new TBP('products', false)
  .find('leaf')
  .tag('hot')
  .sort('public_desc')
  .populate(['images'])
  .paginate('products', 15, 1)
  .then(result => {
    // result: { data, current_page, last_page, per_page, total, ... }
    // Do something for result...
  })
  .catch(error => console.error(error)) // catch 鏈式函式是錯誤處理，非必要
```

* `paginate(model, rows, page)` 為終端方法，內部呼叫 `fetch()` 並回傳 `Promise`。
* 回應處理透過 `Common.responseHandle(response, 'api.fetch.fail', ...)`，成功時回傳 `response.success`（即分頁資料本體）。

***

### `AsyncTBP` React 元件

`resources/assets/js/react/frontend/async_tbp.js`，提供一個「左側父層分類清單、右側內容列表＋分頁」的非同步查詢範本元件。

#### **Props**

<table><thead><tr><th width="121.9296875">Prop</th><th width="124.44921875">型態</th><th>說明</th></tr></thead><tbody><tr><td><code>pointer</code></td><td>string</td><td>起始查詢指標（或 url，見 <code>isUrl</code>）</td></tr><tr><td><code>isUrl</code></td><td>boolean</td><td>是否以 url 查詢</td></tr><tr><td><code>tag</code></td><td>string</td><td>選填，套用於標記查詢（<code>new</code>、<code>hot</code>、<code>sale</code>、<code>custom1</code>、<code>custom2</code>)</td></tr></tbody></table>

#### **行為**

1. 掛載後以 `find('children_cate')` 分頁載入 `pointer` 底下所有「本身仍有子分類」的父層分類（每頁 15 筆，自動翻頁載入到底），顯示於左側清單。
2. 首次載入完成後，預設選取第一個父層分類，並以該分類的 `origin_url`、`find('leaf')`、`tag` 查詢其底下最末層內容，顯示於右側並附上分頁元件（`react/helper/paginate.js`）。
3. 點選左側任一父層分類會重新查詢右側內容列表。
4. 右側分頁元件變更頁碼／每頁筆數時，重新以目前選取的父層分類查詢。

#### **掛載方式**

於站台 script 中呼叫：

```js
import { asyncTBPRender } from '../../react/frontend/async_tbp'

asyncTBPRender('#async_tbp') // #async_tbp 可以替換成其他元素 id 或 class，但一頁只會處發一次
```

{% hint style="info" %}
可以將此範例元件複製到站台的 js 目錄中進行修改，如果是此狀況，可以在站台的 script.js 中用以下方式載入。

```js
import { asyncTBPRender } from './async_tbp'

asyncTBPRender('#async_tbp') // #async_tbp 可以替換成其他元素 id 或 class，但一頁只會處發一次
```
{% endhint %}

並在 Blade 內以 `data-*` 屬性提供初始參數：

```blade
<section id="async_tbp" data-pointer="product" data-is-url="1" data-tag="hot"></section>
```
