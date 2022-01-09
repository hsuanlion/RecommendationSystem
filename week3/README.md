# 協同過濾三種方法實作比較

欲比較的三種協同過濾實作法：
1. 手刻CF User Based
2. 手刻CF Item Based
3. Surprise套件的CF Item Based

## 資料清洗

Ratings
- 新增DATE時間欄位

Metadata
- 新增清洗後的categroy_欄位
- 去重複
- 其餘新增欄位因為沒有在模型中使用到故忽略


## 資料篩選

Metadata
- 篩選資料類別屬於Beauty & Personal Care者。

Ratings
- 篩選每位使用者對同一商品的最新一則評論資料
- 篩選資料年份>2014者
- asin商品代號是屬於Beauty & Personal Care類別下的資料


## 手刻CF User Based 參數與結果

以有歷史紀錄的34筆用戶清單為評估範圍：Recall分數為0.0。

1. 因為預設將training data小於等於2筆的使用者評分資料刪除，所以訓練資料的BASE就從31萬減少為4662筆（1.4%)。 導致說，在投入新的測試資料時，能比對的相似用戶範圍就變少很多。
	- total users before filtering: 314769
	- total users after filtering: 4662
2. 再加上測試資料中的540筆使用者中，只有38位使用者有歷史評分紀錄，而再加上上述第一點的範圍限制，導致34位使用者只有2位有推薦清單結果，且很不幸的推薦清單都沒有中。
3. 在訓練基礎有效資料有限的情況，加上測試資料幾乎都是全新用戶的情況，CF-User Base的效果不甚理想。

## 手刻CF Item Based 參數與結果

以有歷史紀錄的34筆用戶清單為評估範圍：Recall分數為0.17%。

1. 如果改用item based (以training data來說，物品相似度的base多達32009，比之前user base篩選至少要有3則商品評論的參考範圍4600還多)，即使用 item被哪些使用者購買之關係當作推薦依據，只要有至少一筆商品評論的用戶就可以有item based的推薦清單，所以擁有推薦清單的用戶會比user based更多(item base result: 26 out of 34; user base result: 2 out of 34)。
2. 但使用consine similarity計算item相似度的結果，如果商品屬於熱銷商品，用戶又沒有進行分組降維，item_similartiy矩陣的consine Cosine Similarity（餘弦相似度）幾乎清一色都是1（即每件商品的都極相似），表示用原始的用戶評分，不具有商品鑑別度。
3. 評估結果也顯示，34個測試用戶清單的Recall只有0.17%，還是不慎理想。
4. 若要優化CF item based, 應該著手於user降維。

## Surprise套件的CF Item Based 參數與結果

NOTE: 為了讓surprise在collab跑的動，有再另外縮減Trainings data為2017-09月以後的範圍。

以有歷史紀錄的34筆用戶清單為評估範圍：Recall分數為0.23%。

1. 使用套件執行user base協同過濾的結果，一樣限制至少要有三筆評論紀錄，有清單的用戶數有變多(surprise套件：13 out of 34, 手刻user based: 2 out of 34)。
2. 且雖然使用的training data scope更少(前面兩個方法用2014以後的資料，此法則使用2017-09月以後的資料)，清單的 Recall也是三個方法中最高的0.34%。所以推測，如果換一台更好的雲端機器，跑2014以後的training data，是有機會得到更好的Recall結果的。
