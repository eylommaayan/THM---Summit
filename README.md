<img width="761" height="371" alt="image" src="https://github.com/user-attachments/assets/93c4f785-09db-42ee-9ba0-04cd5f2d95de" /># THM---Summit
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
### הדגל שהתקבל![Uploading image.png…]()

