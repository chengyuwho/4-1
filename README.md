<h1>4+1視圖建模及架構設計工程實踐</h1>
<h2>組員:11124228 黃丞語、11124206 施彥愷</h2>
<h1>01 前言</h1>
  
架構設計建模的目的是通過统一的UML語言，完成業務的梳理，並對業務系统進行合理的组組織(分層、分模塊），以提高系统的可擴展性、可重用性、可移植性、易理解性和易測试性，從而達到一個高質量属性的軟件系统。

架構設計的關鍵內容主要包含以下幾個方面：

劃分邏輯視圖：確定領域；

選擇架構風格：確定領域的架構風格，如單體還是分布式；

梳理領域之間依賴關係，並進行適當抽象（隔離依賴）；

技術選型（可選）。

前置條件：在進行架構設計之前，我們需要先梳理清楚業務流程，並使用UML統一語言來呈現統一的組織價值目標。通過清晰的圖標，我們可以更好地傳達信息，一圖勝千言。
<h1>02 架構建模簡介</h1>
<h2>1、"4+1"視圖組織建模</h2>
本文主要介绍“4+1”視圖架構建模的過程，先看看“4+1”視圖模型定義：

![image](https://github.com/chengyuwho/4-1/blob/58a02ae8d5eb95601520de63dce1b1578c680265/4%2B1%E8%A6%96%E5%9C%96%E7%B5%84%E5%BB%BA%E6%A8%A1.png)

“4+1”視圖是描述邏輯架構的重要工具，最早由Philippe Kruchten提出。目前“4+1”視圖已成為主流的架構設計標準。本文中，我們將聚焦用例視圖對業務流程的梳理（如何進行有效的組織業務建模）以及邏輯視圖、開發視圖的分層架構開發代碼實踐。</tr>

組織業務建模是關鍵的業務梳理環節，在“4+1”視圖中，用例視圖需要準確表達出組織的業務用例和系統用例。因此，在開始之前，我們先對用例圖和UML的一些要點進行準備。

<h2>2、業務用例和系统用例的區分</h1>
<h2>2.1 業務用例</h2>
業務用例的定義是指業務執行者希望通過和所研究組織進行交互獲得價值。在定義業務用例时，我们需要將視角放在組織外部和執行者身，關注他們希望從組織中獲得什麼價值，而不是僅僅關注組織能提供什么（注：這個價值不是指組織能提供什麼，而是指執行者想要什麼）。</tr>

以小區物業的充電樁管理中心為例，其業務執行者主要是業主。儘管小區物業可以提供投訴等服務，但業主對充電樁管理中心最大的需求是充電服務，因此，「業主→充電」就是物業充電樁管理中心的業務用例，提供充電服務則是該組織的最大價值所在。

![image](https://github.com/chengyuwho/4-1/blob/8c82076dc5da5c12ea14d4f44631fb0d1c171e8b/%E6%A5%AD%E5%8B%99%E7%94%A8%E4%BE%8B.png)

<h2>2.2 系統用例</h2></tr>
系統用例的定義是指系統能夠為執行者提供的、涉眾可以接受的價值。為了解釋這個概念，我們需要先明確：

<h2>1、系統</h2>

指封裝了自身的數據和行為，並能獨立對外提供服務的實體。一個系統可能包含多個子系統。

<h2>2、執行者</h2>

指在研究系統外，與該系統發生功能性交互的其他人或系統。

<h2>說明</h2></tr>
系统執行者一定是在系统外的，可以是人或者其他系统；</tr>

系統執行者必須與系統有交互；</tr>

系統執行者不一定是業務執行者。</tr>

<h2>3、涉眾</h2>

主要指與要建設的業務系統相關的一切人和事，包括系統執行者在內。</tr>

以小區物業為例，當業主需要進行公區設備報修時，他們會通過微信告訴管家在小區什麼地方、哪個設備有問題需要修理，然後管家通過企微對話框提交報修單。研究該場景的報修下單系統，我們可以得出：</tr>

<h2>系统執行者</h2>
管家。盡管業主是物業服务組織的業務執行者，但直接與報銷下单系統進行交互的是管家，因此管家是該系统的系统執行者。

<h2>涉眾</h2>
包括業主、管家、工程人員和項目經理等等，因為他们都是報修下单系統的利益關係者。

![image](https://github.com/chengyuwho/4-1/blob/d7a79d4d050a48d2e6d14be8d2b17567e5745c59/%E6%B6%89%E7%9C%BE.png)

因此，從上述分析可以得出提交報修單和查看報修單滿足系統用例的概念。以下是該系統用例圖：</tr>

![image](https://github.com/chengyuwho/4-1/blob/d396db799a8cd45ea3c5d229d7447c1055f2e675/%E4%B8%8B%E5%96%AE%E7%B3%BB%E7%B5%B1.png)

與業務用例不同，在研究系統用例時，我們需要將視角切換到系統本身，從系統的角度出發，考慮系統能夠為執行者提供什麼樣的價值，並且該價值是利害關係人都可以接受的。

<h2>2.3 用例UML關係</h2>
系統用例圖中關係主要有四種，分別是關聯、包含、擴展、泛化，如下所示：

![image](https://github.com/chengyuwho/4-1/blob/fcd48f5855f87cd58a5751b3cfd8c87cf53736fb/%E7%94%A8%E4%BE%8BUML%E9%97%9C%E4%BF%82.png)
<h1>03 架構建模的工程實踐</h1>
業務需求背景：為滿足公司對社區物業電梯設備的統一線上管理需求，包括設備台賬、維修管理以及設備工單執行作業標準等業務流程，我們計劃進行電梯監管系統的建設。</tr>

![image](https://github.com/chengyuwho/4-1/blob/48780f3eedb61fffe7be638be1a3ef8ac79a620e/%E6%9E%B6%E6%A7%8B%E5%BB%BA%E5%A9%86%E5%B7%A5%E7%A8%8B%E5%AF%A6%E8%B8%90.png)

<h2>1、用例視圖：業務用例、系统用例</h1>
從需求背景中，我們可以看到電梯設備需要進行大量的信息管理，包括維護、修理、設備記錄以及統一的外部供應商如何管理維修電梯的標準。為了保障電梯的安全使用，我們需要將視角放在組織外部，關注執行者（安全監管員）的需求和價值。

通過分析，我們可以看出該系統的核心價值是確保電梯能夠安全使用，防止出現重大故障隱患和安全事故。基於此，我們可以進行業務用例和系統用例的梳理：

![image](https://github.com/chengyuwho/4-1/blob/68eb386d12d47f53f940be9040efd8d25b9615f2/%E6%A5%AD%E5%8B%99%E7%94%A8%E4%BE%8B%E7%B3%BB%E7%B5%B1%E7%94%A8%E4%BE%8B.png)

<h2>2、流程視圖</h2>
我們對電梯監管的每個系統用例（業務的靜態）進行了拆解，並分析了每個場景的業務時序圖（業務的動態）。在設計系統運行時，我們重點關注了實際業務流程中的系統交互。通過直觀呈現完成系統用例的動態交互過程，我們可以清楚地看到涉及的用戶角色和外部系統。以下是電梯工單維保的業務時序圖示例：

![image](https://github.com/chengyuwho/4-1/blob/1c7ff31201632d030adf3bba225fddd0a45449e0/%E6%B5%81%E7%A8%8B%E8%A6%96%E5%9C%96.png)

<h2>3、邏輯視圖</h2>
邏輯視圖是面向系統邏輯分析和設計的，用於描述系統邏輯結構的視圖。在電梯監管系統中，我們採用領域模塊為核心，基於六邊形分層架構思想的專案模塊分析模型。其中，domain-context領域模塊可以拆分為核心域、支撐域和通用域模塊。這些模塊的劃分將在後續的工程編碼環節中得到體現：

![image](https://github.com/chengyuwho/4-1/blob/83d4ba0099f7893655e2db80dc34ad4ff20f317b/%E9%82%8F%E8%BC%AF%E8%A6%96%E5%9C%96.png)</tr>

這裡做一下補充：我們經常說的DDD（領域驅動設計）會聚焦在領域建模上，因為領域建模是領域驅動設計的核心。常見的領域建模方法包括事件風暴建模和四色建模法。

<h2>4、部署視圖</h2>
部署視圖主要面向系統部署，描述系統的交付、安裝和部署的過程。它解決了系統安裝部署的問題，展示了系統的交付、安裝和部署關係。在我們系統中，所有的實現都在阿里雲K8s進行Pod容器部署。這裡不做展開。

<h2>5、編碼的環節-系统工程應用</h1>

在工程實踐中，我們科研架構委員會採用了六邊形架構。該架構抽象了核心的領域層，並使用適配器模式解耦對外的層次依賴。</tr>

![image](https://github.com/chengyuwho/4-1/blob/530ab1aae8b9cb252bf94c847578780ccc981b2e/%E7%B7%A8%E7%A2%BC%E7%9A%84%E7%92%B0%E7%AF%801.png)</tr>

<h2>基本思想</h2>
保證核心業務邏輯的穩定，領域層應作為最純粹、最少對外依賴的層次，只包含業務知識和業務規則，不過多關心技術細節的實現（如數據持久化存儲、第三方接口調用、推送消息等）。</tr>

根據我們以上“4+1”業務建模，可以進行子域的劃分。根據子域對於業務重要性的不同，可以分為核心域、支撐域和通用域。在適配器模塊，核心域是以電梯為主體的領域。</tr>

核心域：電梯域</tr>
支撐域：工單域</tr>
通用域：用戶域</tr>

![image](https://github.com/chengyuwho/4-1/blob/8777eee2bf99f8eb679b90986eda260c3b013e12/%E7%B7%A8%E7%A2%BC%E7%9A%84%E7%92%B0%E7%AF%802.png)

<h2>5.1 領域層</h2>
分層架構以領域層(domain)為核心來分層建設，領域層包含以下內容（工程結構模塊說明）：

![image](https://github.com/chengyuwho/4-1/blob/fcc79d81027536193c86b0a11573eba894a45cdd/%E9%A0%98%E5%9F%9F%E5%B1%A4.png)

<h2>5.2 核心域代碼示例</h2>

![image]()

部分代碼
<h2>5.3 倉儲層代碼示例</h2>
Repository適配dao實現數據持久化：

![image]()

<h2>5.4 兩個注意點</h2>
<h2>1、關注業務域的劃分</h2>

業務域劃分不清晰的系統往往更容易腐化，因為一旦業務域劃分比較亂，分不清應該放在哪個域的模塊中，再經過時間和人員迭代，系統會愈發的混亂。例如，在我們應用中，有一部分接口定義放在了通用域的服務中，但從業務域劃分上來看，其並不夠通用，因此將其放在通用域的服務是不合適的。我們需要單獨定義專門的業務域的包來管理該業務專門的服務接口。

<h2>2、適度設計</h2>

在設計業務系統時，避免使用表達式的邏輯來實現業務。不要為了追求Configuration而嘗試把所有的業務邏輯以表達式的方式進行表述。如果業務邏輯落在資料庫或者Diamond裡變成片段化的QLExpress或者SPEL，那麼在工程代碼內完全看不懂系統處理了什麼業務邏輯。這樣會導致業務邏輯散落一地，可讀性不高，維護成本也會增加。我們需要回到設計的初衷，設計是為了解決問題的，而不是為單純的炫耀設計。簡單即好，適度設計。

<h1>04 總結與展望</h1>

面向業務進行架構設計建模，將業務知識沉澱到設計模型中。通過對系統的架構建模，可以進行整體的業務流程梳理和第三方系統的數據交互，確認系統建設的價值及目標。同時，在開展編碼前對關鍵的技術模塊、分層結構做好解耦的基礎，指導後續業務代碼的編程。

我們在熟悉建模的流程和理論方法後，期望能夠做到一天甚至半天完成一個系統的業務流程的組織建模。接下來可以快速完成對核心實現模塊的分層設計，進一步完成模塊的領域事件、狀態機、數據持久化等核心代碼實現。這樣可以框定後續的業務編碼的邊界，職責單一，只剩下業務邏輯代碼的實現。
