# SAS_FinalProject_1231
Replicate the paper to practice the SAS 

## Paper內容描述：
  此份Paper題目為： 申購積極性對新上市公司股票績效的影響，作者為 王朝仕(Chao-Shi Wang);陳振遠(Roger C. Y. Chen)，於2008年發佈於管理學報上，此文獻研究目的主要有兩個：
   1. 探討投資人在申購市場之積極性對 新上市(櫃)公司 (IPOs) 上市後績效的影響。
   2. 嘗試解釋影響投資人申購積極性的因素。
   
  作者利用台灣於2000~2003年間的IPOs公司資料，排除櫃轉市、存託憑證、上市首日無交易資料、缺乏公開申購相關資料等樣本後，共以341家IPOs公司進行研究。而資料來源主要源於台灣經濟新報資料庫、公開資訊觀測站與台灣證券交易所。
  
  以下簡要說明相關研究方法，此份研究中最重要的變數即為申購積極性，作者以公開申購中會有的中籤率資料，將其倒數後取ln作為申購積極性的代理變數，並透過持有期間累積報酬與持有期間累積超額報酬作為衡量IPOs公司績效表現的變數，用以探討屬於不同申購積極度的公司在長短期績效表現是否有所差異。最後作者透過市場時機(EX:
  IPOs前十日市場投組報酬、IPOs前一個月其他IPOs公司首日報酬..等)之代理變數，以及其他相關申購資料作為解釋變數，探討何種因子會顯著影響申購積極度，因而得到下段幾種結果。
  
  在此份研究當中，作者發現：
   1. 當投資人對 IPOs 的申購高度積極時，雖然其上市初期的績效表現較佳，但長期投資績效卻會產生虧損。
   2. 投資人申購積極性低的 IPOs 會有較差的上市初期績效，但其長期績效卻將超越投資人申購高度積極的 IPOs。
   3. 承銷價 、市場時機、掛牌交易市場與所屬產業類別等因素，皆顯著影響 IPOs 投資人的申購積極性。
  
  透過此份研究，作者建議在投資IPOs公司時，可買進投資人申購積極性低的 IPOs，如此不但可避開申購市場嚴重資訊不對稱所可能衍生的風險，且更能大幅提升獲利機會。
   
## 以下說明這份Project中所包含的檔案以及Code的用途。

#### CalculateBHR_AR.sas
  由於此研究中需要使用到各IPOs公司在不同期間內的持有期間累積報酬(BHR)以及持有期間累積超額報酬(AR)，因此我們利用TEJ抓取每一家公司於上市(櫃)後三年的日報酬率，以及對應其市場(TSE 或 OTC)的市場日報酬率，進而用以計算BHR 及AR，並將Code存放於CalculateBHR_AR.sas中，另外，需要注意的是，由於樣本中有些於證交所(TSE)上市，有些於櫃買中心(OTC)上櫃，因此，需要先區分出各樣本所屬市場為何，並Merge他們所屬的市場日報酬。

#### DealWithAllot.sas
  此份研究需要中籤率，限制申購量..等資訊，我們從證券商業同業公會當中抓下每一年的上市(櫃)資訊，希望能藉以合併至我們的樣本中，但由於證券商業同業公會下載下來的資料與TEJ上所抓下的IPOs資訊之公司名稱設立方式不同，因此我們利用 DealWithAllot.sas 做初步的資料處理，藉以成功合併所需資訊。
  
#### FirstAndInnitialPriorReturn.sas
  在探討何種因子會影響申購積極性時，作者使用了市場時機作為其中一種解釋變數，市場時機包含三種，分別為IPOs公司上市前一個月其他IPOs公司之首日報酬、期初報酬(上市(櫃)後五天)及IPOs上市前10天市場投組累積報酬，我們將計算其他IPOs之首日報酬以及期初報酬之Code記錄於FirstAndInnitialPriorReturn.sas中，另外，計算此變數之資料來源為TEJ，我們透過TEJ抓取個IPOs公司之上市首日及上市後五日之股價，計算首日及期初報酬，最後將資料輸出後合併。
  
#### Market_Return_Prior_10Days.sas
  市場時機的三種變數中，最後一種即為IPOs上市前10天市場投組累積報酬，為了要計算市場報酬，我們從TEJ中抓取台灣加權指數以及櫃買指數的每日報酬，對應到樣本所屬的市場(若是上市公司使用台灣加權指數，上櫃公司則使用櫃買指數)，並抓取各IPOs公司上市前十日之市場報酬計算其持有期間累積報酬。另外，必須要注意的是，不能以上市日減去十天的範圍作為抓取資料的條件，因為此日期範圍有可能參雜週末或國定假日等未開盤的日期，而導致抓取的資料不滿足10日，此問題的解決方法為：我們以各IPOs公司的上市日作為基礎日期，設定其為 1，並往前數十筆市場報酬資料，因為TEJ中的資料若是遇到國定假日等未開盤時會直接跳過，並不會以空值顯示在資料內，因此可以簡單地以此方法確保每一家IPOs公司都能抓足10筆報酬資料藉以計算上市(櫃)前十日之持有期間累積報酬。
  
#### CreateTable.sas
  上述的變數都製作完成後，我們就可以將變數合併成一份資料檔，用以建立文獻中所需得表格以及圖，另外，由於此份研究中的產業變數使用的為舊式的產業分類，因此我們利用proc format將產業分類轉換成舊式的產業分類，另外，在此份研究中，我們需要將資料區分成高中低三組不同的積極度，故我們也利用Proc format製作自製的format以利後續計算分組後的持有期間累積報酬。在合併且套上所需要的Format後，我們就可以製作文獻中所需要的各個表格。值得注意的是，在製作Table 8 時，需要建立15個迴歸模型，因此我們利用macro 建立function且利用Do loop 讓SAS能便利的完成15組迴歸模型，也減少重複性Code的撰寫。
 
#### DrawPlot.sas
  在此份研究中，作者有以折線圖表示不同積極度群組的平均持有期間報酬，因此，我們利用SAS中的proc sgplot 來建立需要的圖，並將Code儲存於 DrawPlot.sas中。


  
  
  
  
  
  
  
  
  
  
  
  
  
