---
layout: post
title: "Automated, Large-Scale Analysis of Cookie Notice Compliance: Correcting Biases from Previous Studies"
abstract: "Privacy regulations such as the General Data Protection Regulation (GDPR) require websites to inform EU-based users about non-essential data collection and to request their consent to this practice. Many websites violate these regulations as previous studies have documented. However, these studies have significant limitations regarding the general compliance picture: they are either restricted to a subset of notice types, detect only simple violations using prescribed patterns, or analyze notices manually. Hence they are restricted both in their scope and inability to analyze violations at scale.

We present the first large-scale automated analysis of cookie notices that is able to interact with cookie notices, e.g., by navigating to their settings. We present a method that automatically navigates through cookie notices, observes declared processing purposes and available consent options using Natural Language Processing (NLP), and compares the cookies websites use with those they declare. For contrast, previous studies focused on specific Consent Management Platforms (CMP) and, as we show, are biased. We correct for this bias and give an updated report on a variety of potential violations on a set of 35k websites popular for EU residents. For example, we find that 67.5% of websites likely collect users' data despite explicit negative consent."
authors: Ahmed Bouhoula, Karel Kubicek, Amit Zac, Carlos Cotrini, and David Basin
publisher: Submitted to USENIX Security, 2023.
categories: FLoC privacy
keywords: cookies, CMP, GDPR, privacy
---

# Automated, Large-Scale Analysis of Cookie Notice Compliance: Correcting Biases from Previous Studies

**Authors: Ahmed Bouhoula, Karel Kubicek <karel.kubicek@inf.ethz.ch>, Amit Zac, Carlos Cotrini, and David Basin**

**Abstract:** *Privacy regulations such as the General Data Protection Regulation (GDPR) require websites to inform EU-based users about non-essential data collection and to request their consent to this practice. Many websites violate these regulations as previous studies have documented. However, these studies have significant limitations regarding the general compliance picture: they are either restricted to a subset of notice types, detect only simple violations using prescribed patterns, or analyze notices manually. Hence they are restricted both in their scope and inability to analyze violations at scale.*

*We present the first large-scale automated analysis of cookie notices that is able to interact with cookie notices, e.g., by navigating to their settings. We present a method that automatically navigates through cookie notices, observes declared processing purposes and available consent options using Natural Language Processing (NLP), and compares the cookies websites use with those they declare. For contrast, previous studies focused on specific Consent Management Platforms (CMP) and, as we show, are biased. We correct for this bias and give an updated report on a variety of potential violations on a set of 35k websites popular for EU residents. For example, we find that 67.5% of websites likely collect users' data despite explicit negative consent.*

* Download author pre-print of the paper: [PDF](todo)
* Request access to source code and interactive results visualization: [Google form](todo)
* Ahmed Bouhoula's MSc thesis about FLoC: [PDF](https://www.research-collection.ethz.ch/bitstream/handle/20.500.11850/575741/BouhoulaAhmed.pdf?sequence=1&isAllowed=y)

### BibTex

```latex
TBA
```

### Motivation

With the ePrivacy Directive and GDPR, the EU tried to address ubiquitous tracking on websites. Therefore are websites now asking for consent with these practices, often framed as "cookie consent." As numerous prior studies, including our own [CookieBlock](https://karelkubicek.github.io/post/cookieblock), showed, these notices fail the task to inform users and even more in giving users choices.
 <!-- This creates a false expectation of compliance while also harming web usability. -->

These past studies are however significantly constrained, giving us biased measurements. Either they depend on specific technologies present in a subset of consent notice or they are manual and therefore cannot scale. Our work addresses these constraints.

### Crawler interacting with notices

The past automated studies relied on the API or cookies used by specific notices. We created a crawler that detects notices using a crowd-sourced [EasyList Cookie](https://secure.fanboy.co.nz/fanboy-cookiemonster.txt). Then it does not stop at the notice front page, but it navigates the notice as a graph using DFS, observing what interaction is available. This allows us to perceive the notices as users do, not as the API specifies them.

We then browse the website after different interactions with the notice: rejecting or accepting the cookies, saving default settings, dismissing the notice with a close button, or not interacting at all.

Using machine translation and multilingual models, we support websites in the following languages: Danish, Dutch, English, Finnish, French, German, Italian, Portuguese, Polish, Spanish, and Swedish.


*Below, we show you an example interactive cookie notice. All elements in blue dotted boxes contain descriptions of how our ML models would classify them, which is available when you hover your mouse over them. You need JS to see the demo.*

<!-- interactive notice -->
<div>
<style type="text/css" scoped>
* {
  box-sizing: border-box;
}

#regForm {
  background-color: #f7f7f7;
  margin: 30px auto;
  font-family: Raleway;
  padding: 40px;
  padding-bottom: 80px;
  width: 40%;
  min-width: 520px;
}

h1 {
  text-align: center;  
}

/* Hide all steps by default: */
.tab {
  display: none;
}

button {
  background-color: #04AA6D;
  color: #ffffff;
  border: none;
  padding: 10px 20px;
  font-size: 17px;
  font-family: Raleway;
  cursor: pointer;
}

#close {
  background-color: #f7f7f7;
  color: #000000;
  border: none;
  padding: 10px;
  font-size: 30px;
}

button:hover {
  opacity: 0.8;
}

#rejBtn {
  background-color: #fe8184;
}
#setBtn {
  background-color: #2ab5f6;
}
#savBtn {
  background-color: #2ab5f6;
}
#cloTool {
  margin-top: -40px;
  margin-right: -30px;
  float:right;
}

.tooltip {
  position: relative;
  display: inline-block;
  border: 1px dotted rgb(0, 0, 255);
  padding: 3px;
}

.tooltip .tooltiptext {
  visibility: hidden;
  width: 20em;
  background-color: black;
  color: #fff;
  text-align: center;
  border-radius: 6px;
  padding: 5px 0;
  /* Position the tooltip */
  position: absolute;
  z-index: 1;
  top: 100%;
  left: 50%;
  margin-left: -9em;
}

.tooltip:hover .tooltiptext {
  visibility: visible;
}
</style>

<form id="regForm">
  <span class="tooltip" id="cloTool">
    <button type="button" id="close" onclick="nextPrev(2, 'close')"><span aria-hidden="true">&times;</span></button>
    <span class="tooltiptext">Interactive element prediction: close.</span>
  </span>
  
  <h1>Example cookie notice</h1>
  <div class="tab">
    <p>
      <span class="tooltip">
        We use cookies to enhance your browsing experience. <span class="tooltiptext">NLP prediction: Essential purpose.</span>
      </span>
      <span class="tooltip">
        We also use them to serve personalized ads and to analyze our traffic. <span class="tooltiptext">NLP prediction: Non-essential purpose.</span>
      </span>
    </p>
  </div>
  <div class="tab">
    <p>Choose what types of cookies you consent to. Limitation of our study is that we do not inspect what checkbox/toggle values were selected by users. We however submit the default value, and if we observe cookies, it means that consent was not active.</p>
    
    
    <span class="tooltip">
      <input type="checkbox" checked="true" disabled=""> <label>Essential cookies are required to enable essential functions of the website, such as registration or shopping carts. They are always enabled to allow for a smooth and problem-free browsing experience.</label><span class="tooltiptext">NLP prediction: Essential purpose.</span>
    </span>
    
    <span class="tooltip">
      <input type="checkbox"> <label>Advertising cookies tailor advertisements to the viewer, help track the user and collect sensitive data of the user's browsing behavior, usually across multiple websites.</label><span class="tooltiptext">NLP prediction: Non-essential purpose.</span>
    </span>
    
  </div>
  <div class="tab">
    <p>Now the crawler would randomly browse 5 links, scroll, observing all notice after <span class="consentType" style="font-weight: bold;"></span> consent.</p>
  </div>
  <div style="float:right;">
    <span class="tooltip" id="rejTool">
      <button type="button" id="rejBtn" onclick="nextPrev(2, 'reject')">Reject all</button>
      <span class="tooltiptext">Interactive element prediction: reject.</span>
    </span>
    <span class="tooltip" id="setTool">
      <button type="button" id="setBtn" onclick="nextPrev(1)">Customize</button>
      <span class="tooltiptext">Interactive element prediction: settings.</span>
    </span>
    <span class="tooltip" id="savTool">
      <button type="button" id="savBtn" onclick="nextPrev(1, 'save')">Save your choices</button>
      <span class="tooltiptext">Interactive element prediction: save.</span>
    </span>
    <span class="tooltip" id="accTool">
      <button type="button" id="accBtn" onclick="nextPrev(2, 'accept')">Accept all</button>
      <span class="tooltiptext">Interactive element prediction: accept.</span>
    </span>
    <button type="button" id="resBtn" onclick="nextPrev(-2)">Reset demo</button>
  </div>
</form>

<script>
var currentTab = 0; // Current tab is set to be the first tab (0)
showTab(currentTab); // Display the current tab

function showTab(n) {
  var x = document.getElementsByClassName("tab");
  x[n].style.display = "block";
  if (n == 0) {
    document.getElementById("rejTool").style.display = "inline-block";
    document.getElementById("cloTool").style.display = "inline-block";
    document.getElementById("accTool").style.display = "inline-block";
    document.getElementById("setTool").style.display = "inline-block";
    document.getElementById("savTool").style.display = "none";
    document.getElementById("resBtn").style.display = "none";
  } else if (n == 1) {
    document.getElementById("rejTool").style.display = "none";
    document.getElementById("cloTool").style.display = "inline-block";
    document.getElementById("accTool").style.display = "none";
    document.getElementById("setTool").style.display = "none";
    document.getElementById("savTool").style.display = "inline-block";
    document.getElementById("resBtn").style.display = "none";
  } else {
    currentTab = 2;
    document.getElementById("rejTool").style.display = "none";
    document.getElementById("cloTool").style.display = "none";
    document.getElementById("accTool").style.display = "none";
    document.getElementById("setTool").style.display = "none";
    document.getElementById("savTool").style.display = "none";
    document.getElementById("resBtn").style.display = "inline";
  }
}

function nextPrev(n, consent = "") {
  var x = document.getElementsByClassName("tab");
  x[currentTab].style.display = "none";
  currentTab = currentTab + n;
  document.getElementsByClassName("consentType")[0].innerHTML = consent;
  showTab(currentTab);
}
</script>
</div>

### ML and NLP models

Crawler extracts the declared behavior by the notice, i.e., its text and interaction choices, as well as the observed behavior, i.e., cookies that were used by the website depending on our notice interaction. We reason about the collected data using three ML models.

* We predict the declared data collection purposes in the notice text using a BERT model. We trained this model using the dataset by [Santos et al.](https://arxiv.org/abs/2110.02597), where we reduced labels to binary decision whether the sentence declares data processing purpose that requires users' consent. This included the following purposes: profiling, advertising, custom content, analytics, and social media features. This model reaches 97.6% accuracy.
* We predict the purpose of interactive elements of the cookie notice using a BERT model trained on a dataset of 2353 interactive elements annotated in notices by us. The labels are accept, reject, close, save, settings, and other. This model achieves 95.1% accuracy.
* We classify whether the website uses cookies that require consent using an XGBoost model. This model is based on our prior [CookieBlock](https://karelkubicek.github.io/post/cookieblock) work, but it operates over all cookies of website, allowing us to classify when website uses such cookies with a precision of 98.67% and a recall of 91.82%.

<!-- ML perf? -->

### Violation detection

We select 35k websites for crawling using Chrome UX report. This list generalizes the real browsing patterns the best ([Ruth et al.](https://doi.org/10.1145/3517745.3561444)). Selecting EU (+UK) countries that speak one of the supported languages ensures that the websites target users under GDPR.

The crawled data allows us to reason about differences between declared and observed behavior. The outputs of ML models are parameters for or decision tree, which outputs ten privacy violations or dark patterns.

![Decision tree for violations](https://karelkubicek.github.io/assets/images/gen-cookies/decision_violations.svg)
*Decision tree that takes as input the classifications by our model and outputs all types of potential violations present on the website.*

![Decision tree dark patterns](https://karelkubicek.github.io/assets/images/gen-cookies/decision_darkpatterns.svg){: style="float: left; width:50%"}![Observed violations](https://karelkubicek.github.io/assets/images/gen-cookies/violations_bar.svg){: style="float: left;width:50%;"}
*On the left, the decision tree of dark patterns. On the right, observed statistics of potential violations and dark patterns.*

### Selected results of specific populations

We can parametrize website selection based on the country, popularity rank, and the consent notice technology the website uses. We list the most important observations, all of them are statistically significant:

* Popular websites have fewer **visible violations** such as missing notice, missing reject button, or undeclared purposes than less popular websites. Yet when it comes to how they **track users** the situation is the opposite. Popular websites ignore rejected consent, assume consent before user interacts with the notice, or assume consent after user uses "close button" more often than less popular websites.
* Sampling bias of prior studies, stemming from reliance on specific consent notice technologies, makes the majority of observed results vastly different from Chrome UX report sample. Specifically, websites using [Transparency & Consent Framework](https://iabeurope.eu/transparency-consent-framework/) studied by [Matte et al.](https://doi.org/10.1109/SP40000.2020.00076) are far more violating consent.

![Violations per rank](https://karelkubicek.github.io/assets/images/gen-cookies/violations_bar_per_rank.svg){: style="float: left; width:50%"}![Violations bias comparison](https://karelkubicek.github.io/assets/images/gen-cookies/violations_bias.svg){: style="float: left;width:50%;margin-bottom:20px;margin-top:20px;"}
*On the left, we present violations per rank from the Chrome UX report. On the right, we compare our results with other studies and investigate whether their website selection caused any bias.*



### Q&A

* **Q:** How can I access the study results or code?

  **A:** Since our work depends on ML, it can produce false positives. To reduce the impact of false reporting of individual websites violating regulations, we restrict access upon request. Please fill [this form](TODO) to get access.

* **Q:** What is the probability that your violation decision tree produces false positives?

  **A:** We tuned the models to be conservative, so they rather produce false negatives. Our evaluation on 200 random websites confirmed that the predictions behave according to our plan, for more details, check Section 7 of the paper.


### Acknowledgement

The authors would like to thank:

 * Nataliia Bielova and Vincent Toubiana from CNIl for their feedback on our project.

### Updates

* *February 27, 2023:* The initial version of this page.
