這是一些常見的 scala style 的問題，也能夠使用 http://www.scalastyle.org/ 被自動偵測出來

## Scala 常見的問題

1. 避免型別轉換和測試

   對於作業或是專案上，千萬不要使用 isInstanceOf 或 asInstanceOf, 總是會有更好的方法.如果你發現你自己想要使用 casts, 請退後一步，思考你嘗試想要達到什麼? 並重新閱讀作業的說明和對應的教學影片

2. 縮排
   確認你的程式碼有完整的縮排，他會變得可讀性更好

   這可能看起來微不足道，與我們的練習不是很相關，但想像你自己在未來是一個團隊的一部分，與其他編程人員工作在相同的文件：非常重要的是，每個人都尊重風格規則，以保持代碼健康。

   如果你的編輯器沒有按你想要的方式縮進，你應該知道如何改變它的設置。 在Scala中，標準是縮進使用2個空格（不要用 tabs）。

3. 行長度和空格

    確保你程式碼不要太長，否則將會變得很難閱讀。
    宣告一些局部變數綁定而不是使用很長的行數代碼，使用一致的空格會讓你的程式碼可讀性更高

    Example:

    過長的程式碼和缺少空白符號
    ```
    if(p(this.head))this.tail.filter0(p, accu.incl(this.head))else this.tail.filter0
      (p, accu)
    ```

    較好的方式:
    ```
    if (p(this.head))
      this.tail.filter0(p, accu.incl(this.head))
    else
      this.tail.filter0(p, accu)
    ```
    
    更好的方式:(請看第4點和第6點)
    ```
    val newAccu = 
      if (p(this.head)) accu.incl(this.head)
      else accu  
    this.tail.filter0(p, newAccu)
    ```

4. 使用本地值(local values)來簡化複雜的表達示(expressions)

    當使用函數風格來撰寫代碼時，methods 通常都會被實作成 function call 的組合。若這樣被合併的表達示變得太大時，代碼將會變得很難閱讀。(詳見第5點)

    像這樣的案例在傳遞他們到 function 時，將他們儲存在本地值的參數是比較好的但要確保本地值有個有意義的名稱


5. 對於 method 或 Values 要取個有意義的名稱
Method, fields 和 values 在選擇名稱時必須要很小心，以便讓代碼容易閱讀。
Method 名字必須要清楚表示出這個 method 在做什麼。No 或 temp 就永遠都不會是好的名字 :)

    下面這些是可以改進的例子
    ```
    val temp = sortFunction0(list.head, tweet) // sortFunction0 是在做什麼呢?
    def temp(first: TweetSet, second: TweetSet):TweetSet = ...
    def un(th: TweetSet, acc: TweetSet): TweetSet = ...
    val c = if (p(elem)) accu.incl(elem) else accu
    def loop(accu: Trending, current: TweetSet): Trending = ...
    def help(ch: Char, L2: List[Char], compteur: Int): (char, Int) = ...
    def help2(L: List[(char, Int)], L2: List[Char]): List[(Char, Int)] = ...
    ```

6. 常見的子表達示

    你必須要避免不必要的計算密集性的 method, 例如:
    ```
    this.remove(this.findMin).ascending(t + this.findMin)
    ```

    呼叫 `this.findMin` 兩次，如果每一次的呼叫都是昂貴的並且又都沒有副作用(side-effect), 你可以藉由 local variable 來節省一次的呼叫

    ```
    val min = this.findMin
    this.remove(min).ascending(t + min)
    ```

    如果 function 是貝遞迴呼叫，這就變得更重要了:在這個案例中，method 不只被呼叫多次，而是指數型的次數

7. 不要複製-貼上代碼!

    複製貼上對於壞風格總是有警告信號，有許多的缺點:
        
    + 代碼若太長就需要花更多的時間來了解他
    + 如果有兩個部分不相同卻又很相似，這會很難發現差異(下面有範例)
    + 維護兩份代碼並且要確保一致，是很容易出錯的
    + 如果要修改，工作量是倍增的
    + 你必須將共同的部分分解成單獨的 method 而不是複製代碼

    例如:
     ```
     val googleTweets: TweetSet = TweetReader.allTweets.filter(tweet =>  google.exists( word => tweet.text.contains(word)))
     val appleTweets: TweetSet = TweetReader.allTweets.filter(tweet => apple.exists(word => tweet.text.contains(word)))
     ```

    將代碼改寫成下面這樣更好:
    ```
    def tweetsMentioning(dictionary: List[String]): TweetSet = 
      TweetReader.allTweets.filter(tweet => 
        dictionary.exists(word => tweet.text.contains(word)))
    
    val googleTweets = tweetsMentioning(google)
    val appleTweets = tweetsMentioning(apple)
    ```

8. scala 不需要分號

9. 不要將 println statement 提交

    您應該清理您的代碼，並刪除所有 `print` 或 `println` 語句，然後提交它。當你為一個公司工作並創建在生產中使用的代碼時，同樣的情況將適用：最終代碼應該沒有調試語句。

10. 避免使用Return

    在Scala中，您通常不需要使用顯式返回，因為控制結構（如if）是表達式。例如，在

    ```
    def factorial(n: Int): Int = {
      if (n <= 0) return 1
    else return (n * factorial(n-1))
    }
    ```
    其中 return 語句可以簡化並刪除。

11. 避免可變的局部變數(var)

    由於這是一個關於函數式編程的課程，我們希望你習慣於用純粹的函數式編寫代碼，而不使用副作用操作。你經常可以將使用可變局部變量的代碼重寫為使用帶有累加器的輔助函數的代碼。下面這段代碼

    ```
    def fib(n: Int): Int = {
      var a = 0
      var b = 0
      var i = 0
      while (i<n) {
        val prev_a = a
        a = b
        b = prev_a + b
        i = i + 1
      }
      a
    }
    ```
    可以被這段代碼取代
    ```
    def fib(n: Int): Int = {
      def fibInter(i: int, a:Int, b:Int): Int = 
        if (i == n) a else fibInter(i+1, b, a+b)
      fibInter(0, 0, 1)
    }
    ```

12. 消除多餘的 if 表達示

    這段代碼
    ```
    if (cond) true else false
    ```
    可以簡化成這段
    ```
    cond
    ```