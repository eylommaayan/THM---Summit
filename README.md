# THM---Summit
תרגיל מעשי בתחום הנדסת זיהויים (Detection Engineering) וסימולציית תקיפה מול תוקף מתקדם (Sphinx), הממחיש את העלייה בשלבי **פירמידת הכאב (Pyramid of Pain)**[cite: 2, 6].
<img width="1497" height="416" alt="image" src="https://github.com/user-attachments/assets/f5f09676-3dfb-4a6e-a4cc-a79bb7e3b27e" />


# TryHackMe – Summit Room Walkthrough & Lab Report

תרגיל מעשי בתחום הנדסת זיהויים (Detection Engineering) וסימולציית תקיפה מול תוקף מתקדם (Sphinx), הממחיש את העלייה בשלבי **פירמידת הכאב (Pyramid of Pain)**[cite: 2, 6].

---

## שלב 1: חסימת דוגמית ראשונה באמצעות ערכי גיבוב (Hash Values)

### תרגום הודעת התוקף (Update: You Blocked Me!)
> "היי שוב,  
> עבודה טובה. הזיהוי שהוספת מנע מהנוזקה שלי לרוץ. מכיוון שערכי גיבוב (Hashes) של קבצים הם ייחודיים לכל קובץ, הם נחשבים לאינדיקטורים בעלי רמת המהימנות הגבוהה ביותר (*Highest confidence*) — אתה יכול להיות בטוח לחלוטין שאם תראה את ה-Hash הזה שוב, מדובר בנוזקה שלי.  
>   
> יחד עם זאת, זהו גם אחד החסרונות הבולטים ביותר בהסתמכות על גיבוב בלבד כמנגנון זיהוי[cite: 6]. מכיוון שהם רגישים במיוחד לשינויים, מספיק שאשנה ביט בודד בקובץ — וחוק הזיהוי שהגדרת ייכשל לחלוטין[cite: 6].  
>   
> למעשה, כל מה שעשיתי הפעם היה לקמפל מחדש (Recompile) את הנוזקה: נוצר קובץ בעל Hash חדש לחלוטין, והרצתי אותו בלי בעיה[cite: 6]. נראה אם תצליח למצוא דרך חדשה לזהות את `sample2.exe`![cite: 6]"

---<img width="766" height="330" alt="image" src="https://github.com/user-attachments/assets/c9945713-ea3a-4be8-9e0c-38a0cab68ff8" />


### מה בוצע בשלב זה בפועל?
1. **ניתוח דינמי ב-Sandbox:**  
   הקובץ `sample1.exe` הועבר לבדיקה במערכת ה-Malware Sandbox[cite: 2, 3]. בניתוח התקבלו חתימות הקובץ (MD5, SHA1 ו-SHA256) וכן התנהגות ראשונית המעידה על נוכחות של Metasploit[cite: 3].
2. **חילוץ ה-Hash:**  
   חולץ ערך ה-SHA256 המלא של הקובץ[cite: 3]:  
   `9c550591a25c6228cb7d74d970d133d75c961ffed2ef7180144859cc09efca8c`[cite: 3]
3. **הגדרת חסימה ב-EDR:**  
   במסך `Manage Hashes`, הוזן ערך ה-SHA256 תחת רשימת ה-Blocklist[cite: 4, 5]. לאחר ההחלה, מנוע ההגנה חסם את הרצת הקובץ על תחנת העבודה[cite: 5].
<img width="780" height="344" alt="image" src="https://github.com/user-attachments/assets/9a3f0ac0-0a3b-4e3b-a7e2-6bd2ec110640" />

---

### הדגל שהתקבל (Flag 1)
```text
THM{f3cbf08151a11a6a331db9c6cf5f4fe4}
<img width="766" height="347" alt="image" src="https://github.com/user-attachments/assets/97fda13f-d74d-41de-83f1-cb954ad831fc" />


```

## שלב 2: חסימת כתובות IP (רמת IP Addresses בפירמידה)

### תרגום הודעת התוקף (Stumped again... for now!)
> "אהה.  
> נראה שעצרת אותי שוב. בטח מצאת את כתובת ה-IP שאליה דוגמית הנוזקה שלי התחברה. חכם!  
>   
> אולם השיטה הזו אינה חסינה לחלוטין — עבור יריב בעל מוטיבציה, זה עניין טריוויאלי לעקוף אותה באמצעות כתובת IP ציבורית חדשה[cite: 13]. בדיוק נרשמתי לספק שירותי ענן וכעת יש לי גישה להרבה יותר כתובות IP ציבוריות![cite: 13]  

<img width="761" height="371" alt="image" src="https://github.com/user-attachments/assets/0c4a651b-0bd1-481b-b580-0399ffc0e4cc" />

> הפעם תצטרך לזהות את `sample3.exe` בדרך אחרת[cite: 13]. כבר הרמתי את השרת שלי מכתובת IP חדשה ויש לי עוד שרתי גיבוי רבים למקרה שהם ייחסמו![cite: 13]  
> בהצלחה. 😈"[cite: 13]

---
<img width="767" height="362" alt="image" src="https://github.com/user-attachments/assets/c5c3b0c8-5aab-4d7f-abd6-2ed8de9a82db" />

### מה בוצע בשלב זה בפועל?
1. **ניתוח תעבורת רשת ב-Sandbox:**  
   הקובץ `sample2.exe` נותח בסביבת ההרצה המבודדת[cite: 9]. תחת הלשונית **Network Activity**, זוהתה בקשת HTTP יוצאת (Egress) לתשתית אירוח חיצונית (Intrabuzz Hosting Limited)[cite: 10].
2. **חילוץ ה-IOC (אינדיקטור הפריצה):**  
   חולצה כתובת ה-IP של שרת ה-C2[cite: 10]:  
   `154.35.10.113` (בפורט 4444)[cite: 10].
3. **הגדרת חוק חומת אש (Firewall Rule):**  
   במסך `Firewall Rule Manager` הוגדר חוק חסימה לתעבורה יוצאת<img width="760" height="297" alt="image" src="https://github.com/user-attachments/assets/68b89c22-196d-4a71-b828-f655b7135bcc" />
[cite: 12]:
   * **Type:** `Egress`[cite: 12]
   * **Source IP:** `Any`[cite: 12]
   * **Destination IP:** `154.35.10.113`[cite: 12]
   * **Action:** `Deny`[cite: 12]
   
   החלת החוק ניתקה את ערוץ השליטה והבקרה של הנוזקה ומנעה את השלמת המתקפה.

---

### הדגל שהתקבל<img width="762" height="356" alt="image" src="https://github.com/user-attachments/assets/e7da3715-4642-4d7a-95b7-3f49a29e4e9e" />
 (Flag 2)
```text
THM{2ff48a3421a938b388418be273f4806d}
```

<img width="761" height="371" alt="image" src="https://github.com/user-attachments/assets/93c4f785-09db-42ee-9ba0-04cd5f2d95de" />



## שלב 3: חסימת שמות דומיין (רמת Domain Names בפירמידה)

### תרגום הודעת התוקף (RE: Stumped again... for now!)
> "שלומות שוב,  
> נראה שהצלחת לחסום את הדומיין שלי הפעם מכיוון שכל כתובת IP חדשה שאני מנסה לצוץ ממנה מתגלה מיד. אתה מתחיל לגרום לי לצרות, מכיוון שכעת אני נאלץ לרכוש ולרשום שמות דומיין חדשים ולשנות רשומות DNS. תוקפים מסוימים עשויים להתעצבן מזה ולחפש מטרה אחרת וקלה יותר, אבל אני בעל מוטיבציה להמשיך, כמו רבים אחרים[cite: 19].  
>   
> הפעם — חסימת Hashes, כתובות IP או שמות דומיין כבר לא תעזור לך[cite: 19]. אם ברצונך לזהות את `sample4.exe`, שים לב לארטיפקטים (Artifacts) או לשינויים שהנוזקה שלי משאירה על מערכת הקורבן[cite: 19].  
>   
> בהצלחה[cite: 19]."
<img width="767" height="342" alt="image" src="https://github.com/user-attachments/assets/a37a3186-2e35-4a20-9c18-24030c1bfa98" />

---

### מה בוצע בשלב זה בפועל?
1. **ניתוח בקשות DNS ב-Sandbox:**  
   הקובץ `sample3.exe` נותח בסביבת ההרצה המבודדת[cite: 14]. תחת לשונית **Network Activity** ובקטגוריית **DNS requests**, זוהתה שאילתת רשת פעילה לדומיין זדוני ששימש להורדת רכיב Backdoor[cite: 15]:  
   `emudyn.bresonicz.info`[cite: 15]
2. **הגדרת חוק סינון ב-DNS:**

   <img width="761" height="313" alt="image" src="https://github.com/user-attachments/assets/23805460-bee4-4cbe-80c7-f54869ef3b31" />
 
   במסך `DNS Rule Manager` הוגדר חוק חסימה ייעודי[cite: 16, 18]:
   * **Rule Name:** `Block Malicious Domain`[cite: 16, 18]
   * **Category:** `Malware`[cite: 16, 18]
   * **Domain Name:** `emudyn.bresonicz.info`[cite: 16, 18]
   * **Action:** `Deny`[cite: 16, 18]  
   
   החלת החוק מנעה משרתי הארגון לפתור את כתובת ה-IP של הדומיין, ובכך נחסמה ההורדה והתקשורת לשרת השליטה[cite: 18].

---
<img width="772" height="360" alt="image" src="https://github.com/user-attachments/assets/c2b0eed4-c6fa-4898-aafb-43cfef7d47e0" />

<img width="768" height="318" alt="image" src="https://github.com/user-attachments/assets/da689abb-eb90-4a18-b0a1-cb7aeac088d6" />


### הדגל שהתקבל (Flag 3)
```text
THM{4eca9e2f61a19ecd5df34c788e7dce16}

```

## שלב 4: זיהוי עקבות מארח (רמת Host Artifacts בפירמידה)

### תרגום הודעת התוקף (New Approach)
> "היי.  
> אני לא בטוח מה הצלחת לעשות הפעם, אבל בהחלט תקעת לי מקל בגלגלים של מדגם הנוזקה שלי! בזבזתי המון זמן בניסיון להגדיר מחדש את כלי התקיפה והמתודולוגיות שלי כדי לעקוף את מנגנון הזיהוי שלך – סופר מעצבן!  
> לגרום לצוות שלי לפתח טכניקות חדשות שיוטמעו בכלי היריב דרש השקעת זמן עצומה ועלויות כספיות משמעותיות. טוב שיש לנו תקציב נכבד לפעילות הזו, אבל שחקני איום רבים כבר היו מוותרים ומחפשים קורבן חדש עד עכשיו.  
> סוף סוף יש לי את sample5.exe כדי שתנסה לזהות. הפעם מדובר בגישה שונה. במדגם הזה, כל ה'עבודה הכבדה' וההוראות מתרחשות בשרת ה-Backend שלי, כך שאני יכול לשנות בקלות את סוגי הפרוטוקולים שאני משתמש בהם ואת הארטיפקטים שאני משאיר על המארח. תצטרך למצוא משהו ייחודי או חריג לגבי ההתנהגות של הכלי שלי כדי לזהות אותו.  
> צירפתי את יומני חיבורי הרשת היוצאים מ-12 השעות האחרונות על מכונת הקורבן. אולי זה יעזור לך להצליב נתונים.  
> אני כבר לא יודע מה לעשות אם תצליח לעצור אותי גם ברמה הזו.  
> מ-Sphinx המתוסכל."

---

### מה בוצע בשלב זה בפועל?
1. **ניתוח עקבות מארח (Host Artifacts) ב-Sandbox:**  
   הקובץ `sample4.exe` נותח בסביבת ההרצה המבודדת, ונמצא כי הוא מנסה להשבית את מנגנון ההגנה של מערכת ההפעלה באמצעות עריכת ה-Registry:  

<img width="775" height="214" alt="image" src="https://github.com/user-attachments/assets/afa904ed-cd30-4181-901c-a2e3d4903c07" />
<img width="747" height="396" alt="image" src="https://github.com/user-attachments/assets/b4924282-9a0d-42a4-8f20-8659bb046f4d" />

   * **פעולה שבוצעה:** שינוי ערך מפתח ברישום המערכת לביטול הניטור בזמן אמת של Windows Defender.
3. **הגדרת חוק סיגמא (Sigma Rule Builder):**  
   במסך ה-`Sigma Rule Builder` נבחר מקור היומנים מסוג `Sysmon Event Logs` תחת קטגוריית `Registry Modifications`[cite: 5], והוגדרו הפרטים הבאים[cite: 7]:  
   * **Registry Key:** `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Defender\Real-Time Protection`[cite: 7]  
   * **Registry Name:** `DisableRealtimeMonitoring`[cite: 7]  
   * **Value:** `1`[cite: 7]  
   * **ATT&CK ID:** `Defense Evasion (TA0005)`
   * <img width="748" height="607" alt="image" src="https://github.com/user-attachments/assets/8d7d5763-05b4-41dc-897d-ec33e49e4857" />
   <img width="758" height="306" alt="image" src="https://github.com/user-attachments/assets/f33b8ac7-33f0-4b45-a3d3-513e0ae87188" />



   החלת החוק אפשרה למערכת ה-SIEM לזהות בזמן אמת ולחסום את הניסיון לפגוע ברכיבי האבטחה של המארח[cite: 6].

---

### הדגל שהתקבל (Flag 4)
<img width="772" height="355" alt="image" src="https://github.com/user-attachments/assets/a6c363ac-07fc-492b-96af-98368135c568" />

```text
THM{c956f455fc076aea829799c0876ee399}
```

