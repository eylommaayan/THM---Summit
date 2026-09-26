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

