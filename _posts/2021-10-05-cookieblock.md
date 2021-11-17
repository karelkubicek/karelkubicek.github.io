---
layout: post
title: "Automating Cookie Consent and GDPR Violation Detection"
abstract: "The European Union's General Data Protection Regulation (GDPR) requires websites to inform users about personal data collection and request consent for cookies. Yet the majority of websites do not give users any choices, and others attempt to deceive them into accepting all cookies. We document the severity of this situation through an analysis of potential GDPR violations in cookie banners in almost 30k websites. We identify six novel violation types, such as incorrect category assignments and misleading expiration times, and we find at least one potential violation in a surprising 94.7% of the analyzed websites.

We address this issue by giving users the power to protect their privacy. We develop a browser extension, called CookieBlock, that uses machine learning to enforce GDPR cookie consent at the client. It automatically categorizes cookies by usage purpose using only the information provided in the cookie itself. At a mean validation accuracy of 84.4%, our model attains a prediction quality competitive with expert knowledge in the field. Additionally, our approach differs from prior work by not relying on the cooperation of websites themselves. We empirically evaluate CookieBlock on a set of 100 randomly sampled websites, on which it filters roughly 90% of the privacy-invasive cookies without significantly impairing website functionality."
authors: Dino Bollinger, Karel Kubicek, Carlos Cotrini, David Basin
publisher: To appear in The 31st USENIX Security Symposium (UsenixSec'2022), USENIX, 2022.
categories: cookies CMP GDPR privacy
keywords: cookies, CMP, GDPR, privacy
---

# Automating Cookie Consent and GDPR Violation Detection

**Authors: Dino Bollinger, Karel Kubicek <karel.kubicek@inf.ethz.ch>, Carlos Cotrini, David Basin**

**Abstract:** *The European Union's General Data Protection Regulation (GDPR) requires websites to inform users about personal data collection and request consent for cookies. Yet the majority of websites do not give users any choices, and others attempt to deceive them into accepting all cookies.
We document the severity of this situation through an analysis of potential GDPR violations in cookie banners in almost 30k websites. We identify six novel violation types, such as incorrect category assignments and misleading expiration times, and we find at least one potential violation in a surprising 94.7% of the analyzed websites.*

*We address this issue by giving users the power to protect their privacy. We develop a browser extension, called CookieBlock, that uses machine learning to enforce GDPR cookie consent at the client. It automatically categorizes cookies by usage purpose using only the information provided in the cookie itself. At a mean validation accuracy of 84.4%, our model attains a prediction quality competitive with expert knowledge in the field. Additionally, our approach differs from prior work by not relying on the cooperation of websites themselves. We empirically evaluate CookieBlock on a set of 100 randomly sampled websites, on which it filters roughly 90% of the privacy-invasive cookies without significantly impairing website functionality.*

* Conference page: [Usenix Security 2022](https://www.usenix.org/conference/usenixsecurity22/presentation/bollinger)
* Download author pre-print of the paper: [PDF](https://karelkubicek.github.io/assets/pdf/Automating_Cookie_Consent_and_GDPR_Violation_Detection.pdf)
* Download extended version of paper (Dino Bollinger's Master's thesis): [PDF](https://karelkubicek.github.io/assets/pdf/Dino_Bollinger_Analyzing_Cookies_Compliance_with_the_GDPR.pdf)
* Download presentation: TBA
* See full conference talk: TBA
* Download [datasets](https://zenodo.org/record/5568491), [crawler](https://github.com/dibollinger/CookieBlock-Consent-Crawler), [classifier](https://github.com/dibollinger/CookieBlock-Consent-Classifier), and [violation detection scripts](https://github.com/dibollinger/CookieBlock-Violation-Detection)
* Test your website for violations: TBA
* Try our extension CookieBlock: [Chrome](https://chrome.google.com/webstore/detail/cookieblock/fbhiolckidkciamgcobkokpelckgnnol), [Firefox](https://addons.mozilla.org/en-US/firefox/addon/cookieblock/), [Edge](https://microsoftedge.microsoft.com/addons/detail/cookieblock/mnfolmjlccppcgdeinhidialajfiopcc), [Opera](https://addons.opera.com/en/extensions/details/cookieblock/), or check out the [source code](https://github.com/dibollinger/CookieBlock)
* Give us [feedback on the extension](https://forms.gle/tL21ruvPZq2q218P8)

### Bibtex

```latex
@inproceedings{bollinger2022automating,
  author = {Dino Bollinger and Karel Kubicek and Carlos Cotrini and David Basin},
  title = {Automating Cookie Consent and {GDPR} Violation Detection},
  booktitle = {31st USENIX Security Symposium (USENIX Security 22)},
  year = {2022},
  month = aug,
  pages = {TBA},
  isbn = {TBA},
  publisher = {USENIX Association},
  url = {https://www.usenix.org/conference/usenixsecurity22/presentation/bollinger},
  address = {Boston, MA},
}
```

### Cookie consents are fundamentally broken

Browser cookies are the most common method for tracking the session state of websites, as about 80-90% of websites use cookies for user tracking according to prior works. The EU government has attempted to address this through regulations mandating consent, namely by General Data Protection Regulation (*GDPR*) and the ePrivacy Directive.

Despite the consent requirement, prior works showed that less than half of websites show a cookie banner, and those that show the consent still violate even basic rules. The majority of websites do not adhere to the opt-in requirement of the consent, and >10% store affirmative consent before any user action or store positive consent even though the user rejected cookies. Finally, more than half of the consent banners are designed to nudge users to accept all cookies.

By our analysis, we confirm the lack of GDPR compliance by extending and improving upon past research. We analyze the accuracy of the information displayed on cookie banners using a dataset collected from almost 30k websites. Specifically, we identify incorrect category assignments, misleading cookie expiration times, and assess the overall completeness of the consent mechanism. We define six novel methods to detect potential GDPR violations and extend two methods used in prior works. Of the selected domains, we find that 94.7% contained at least one potential violation. In 36.4%, we found at least one cookie with an incorrectly assigned purpose, and in 85.8%, there was at least one cookie with a missing declaration or missing purpose. 69.7% of sites assumed positive consent before it is given, and 21.3% created cookies despite negative consent. Our results indicate that the problem is more severe than previously indicated.

![Violation types](https://karelkubicek.github.io/assets/images/cookieblock_paper/violations_types.png)
*The number of websites that show the respective type of violation. The first six are novel and have not been explored in prior work.*

We support the legal claims by referring to relevant sections of GPDR and ePrivacy Directive and [Planet49 case](https://europeanlawblog.eu/2019/10/08/planet49-cjeu-judgment-brings-some-cookie-consent-certainty-to-planet-online-tracking/) ruled by the EU Court of Justice.

![Histogram of violations](https://karelkubicek.github.io/assets/images/cookieblock_paper/violations_histogram.png)
*This histogram shows the distribution of violation types per site, with the green bar representing the compliant websites. It does not include repetitions of a single type.*

For the case of missing cookie declarations or purposes, we argue that the issues stem from neglect rather than malice. The cause is likely the lack of enforcement and web administrators who are not sufficiently familiar with the legal requirements. These violations can be addressed by providing regulatory authorities crawlers used in this study to improve enforcement of the GDPR. If you are a regulatory authority or any legal body interested in our work, please contact the authors.

### Client-side mitigation with extension CookieBlock

According to evidence from prior works and our own measurements, cookie consent practices are so often in violation of the GDPR that regulatory authorities cannot hope to keep up. Therefore, we provide users with a tool to enforce cookie consent on their web clients without requiring regulations to be enforced. We develop the browser extension CookieBlock that classifies cookies by purpose, deleting those that the user rejects. In this way, the user can remove over 90% of all privacy-invasive cookies, without having to trust cookie banners. Previous attempts to provide users such control, like the P3P standard, failed due to a lack of willingness of website administrators to implement the functionality. We sidestep this problem by not relying on the cooperation of the websites at all.

In order to train a model for cookie purpose classification, we collected a dataset of 304k labeled cookies from 30k websites using a consent management platform with cookie-to-purpose mapping. We then extract statistically-rich, domain-specific features from multiple attributes, including a name, domain, path, value, expiration timestamp, as well as flags such as the “HttpOnly,” “Secure,” “SameSite,” and “HostOnly” properties.

For cookie classification, CookieBlock uses an ensemble of decision trees model, which is trained using the XGBoost library. We evaluate the model by comparing its performance to that of the [Cookiepedia repository](https://cookiepedia.co.uk/). Cookiepedia assigns purposes to cookies based on their name and was constructed manually for 10 years by human operators. We query this repository for purpose predictions and compare the results to the ground truth. In summary, we find that Cookiepedia achieves a balanced accuracy of 84.7%, while our XGBoost model achieves 84.4%. As such, our model is competitive with human expertise, showing that it is possible to automatically classify cookies by purpose using only the information available in the cookies themselves.

![Cookiepedia performance](https://karelkubicek.github.io/assets/images/cookieblock_paper/cookiepedia_perf.png){: style="float: left; width:50%"}![Our XGBoost model performance](https://karelkubicek.github.io/assets/images/cookieblock_paper/xgboost_perf.png){: style="float: left; width:50%"}

![Confusion matrices](https://karelkubicek.github.io/assets/images/cookieblock_paper/conf_matrices.png)
*Performance comparison of our manually-labeled Cookiepedia to our automated XGBoost model, showing that our model is competitive with human expertise.*

We evaluate CookieBlock on a set of 100 websites to quantify the impact the extension has on the browsing experience. CookieBlock causes no issues on 85% of the sites, minor problems involving non-essential website functions on 8%, and more substantial defects on 7%. The latter involve the user's login status being lost due to the cookie removal. To resolve these problems, the user can selectively define website exemptions, and change the classification of cookies through CookieBlock's interface.

![CookieBlock interface](https://lh3.googleusercontent.com/J8SsI8l8oYuu7ytw5WPhRQzBUWMea6vZAD1ekvHuBo8wlukOfT5nZVJuuFPeSQ9qbbdqPNV930Yc_VWrM0WrRsCqCw=w640-h400-e365-rj-sc0x00ffffff)
*CookieBlock interface consists of a simple popup and settings.*

CookieBlock, the cookie purpose classification model, and the web crawler can help various parties:

* Web administrators can inspect the compliance of their website by detecting undeclared cookies and predicting purposes for currently unclassified cookies.
* Regulatory authorities can inspect the website's compliance.
* Privacy- and web-researchers can use the model to classify the cookie types in follow-up studies.

### Q&A

* **Q:** Is CookieBlock collecting any information from my browser?

  **A:** No, all classification and enforcement happen on the client-side. We also do **not** collect any scientific data, and we adhere to the GDPR. If you want to provide us feedback, please fill out the [feedback form](https://forms.gle/tL21ruvPZq2q218P8), or leave a review in the extension store or send us an email. If you observe a malfunctioning website, send us an email to <cookie.block.extension@gmail.com>, fill an [issue on GitHub](https://github.com/dibollinger/CookieBlock/issues), or make a pull-request with updated [`known_cookies.json`](https://github.com/dibollinger/CookieBlock/blob/main/src/ext_data/known_cookies.json).


* **Q:** CookieBlock breaks website xyz.com. What should I do?

  **A:** Easiest solution is to "Add this site as an exception" in the CookieBlock popup. If the website does not start working immediately, you might need to clear cookies for the website. If even this does not help, you can "Pause cookie removal." No matter what technique you use, let us know about your issues by email to <cookie.block.extension@gmail.com> or an [issue on GitHub](https://github.com/dibollinger/CookieBlock/issues).


* **Q:** Does CookieBlock work outside of the EU?

  **A:** Yes, despite the fact that other countries do not have as advanced privacy regulations as the EU, the classification model works independently of your location. For websites other than in English, the model can have slightly reduced performance, as it cannot extract all meaningful features about the cookie content. However, the most useful features for the classification are language agnostic, so this might have only a minor impact.


* **Q:** Can I use CookieBlock together with other extensions?

  **A:** Yes, we are not aware of any incompatibility with extensions such as uBlock Origin, AdBlock, Ghostery, Privacy Badger, Consent-O-Matic, or I don't care about cookies.


* **Q:** CookieBlock does not remove the cookie banners. How do I get rid of them?

  **A:** We want to keep our extension as simple as possible and with only the purpose of removing the cookies. Recommended extensions that remove the popups are: [Consent-O-Matic](https://github.com/cavi-au/Consent-O-Matic), [I don't care about cookies](https://www.i-dont-care-about-cookies.eu/), or [uBlock Origin](https://ublockorigin.com/) with Annoyances filters (e.g., EasyList Cookie).


* **Q:** My website is in the published dataset, and according to it, it contains violations. Can you redact it?

  **A:** While some of the violation detection methods can produce false positives, given the vast non-compliance, it is likely that your website does violate some of the consent requirements. We also appreciate your effort, given that your website at least uses a CMP giving user choices, which is better than 96% of the Internet. You can contact the CMP or the web developer to inspect what can be improved. Anyway, you can contact us at <cookie.block.extension@gmail.com>, and we can figure out the removal from the dataset.


* **Q:** How severe are the detected violations?

  **A:** While we refer to the violations as *potential*, we do so as only a judicial ruling can provide the legal certainty as to whether they are true legal violations. All of the violations imply that the user consents to incorrect information, and it requires an individual inspection to declare them as neglect or malice. However, severe violations as marking Google Analytics cookies as necessary or unclassified or even omitting to declare them forces users to accept them. In such a case, the website might do even worse than the website without a cookie banner, as it gives users a false impression of choice.


### Updates

* *November 17, 2021:* We received all 3 artifact badges: Artifact Available, Artifact Functional, and Artifact Reproduced.
* *November 11, 2021:* Paper released by USENIX.
* *October 15, 2021:* Camera-ready paper version.
* *September 21, 2021:* The initial version of this page.
