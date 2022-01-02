# week 2 content-based filtering

##  1. Item representation
Q: Item representation 有哪些欄位可以被考慮？
Feature Selection: 為每個 item 抽取出一些特徵，用來定義與代表這個物品

非結構化資料特徵，除了title，亦可加入以下欄位來計算詞頻tfidf，但因為RAM上限12G上限，先使用title + brand來計算物件的similarity score矩陣
1. title
2. description（如果有值得話，只取list第一個）
3. brand
4. rank欄位中的category

結構化資料則有以下特徵，但在這次不會用到
1. price
2. rank_num


## 2.Profile Learning
Q:Profiling Learning 有哪些不同定義「相似」的方法？
定義相似的方法可以是：
1. 物件屬性相似
2. 使用者行為相似
這次推薦會以物件屬性相似為主。


## 3. Recommendation Generation
Q: Recommendation Generation 該如何加總 k 個要推薦的產品 
- 根據資料探索，testing users有93%都是新用戶（即沒有歷史資料可以參考，不能做相似用戶、也無法做相似內容)，所以如果是新用戶，則使用rule-based推薦結果，如果是舊客戶，才使用content-based推薦結果。
