# Feature Flag Management System

Java + JSP + Servlet + JDBC + MySQL + Tomcat वापरून बनवलेला **Feature Flag Management System**.

Feature flags म्हणजे code deploy न करता कुठलाही feature ON/OFF करण्याचा mechanism — 
हा project त्यासाठी साधा CRUD-आधारित dashboard देतो.

---

## 🧱 Tech Stack
- **Backend:** Java Servlets (javax.servlet, Tomcat 8/9 compatible)
- **Frontend:** JSP + JSTL + plain CSS
- **Database:** MySQL (JDBC)
- **Build tool:** Maven
- **Server:** Apache Tomcat 8.5 / 9.x

---

## 📁 Project Structure
```
FeatureFlagManager/
├── pom.xml
├── database/
│   └── schema.sql              -> MySQL table + sample data
└── src/main/
    ├── java/com/featureflag/
    │   ├── model/FeatureFlag.java
    │   ├── util/DBConnection.java
    │   ├── dao/FeatureFlagDAO.java
    │   └── servlet/
    │       ├── ListFlagsServlet.java     (/flags)
    │       ├── AddFlagServlet.java       (/addFlag)
    │       ├── EditFlagServlet.java      (/editFlag)
    │       ├── DeleteFlagServlet.java    (/deleteFlag)
    │       └── ToggleFlagServlet.java    (/toggleFlag)
    └── webapp/
        ├── index.jsp
        ├── flags.jsp            -> main dashboard (list + actions)
        ├── addFlag.jsp
        ├── editFlag.jsp
        ├── css/style.css
        └── WEB-INF/web.xml
```

---

## ⚙️ Setup Steps

### 1. MySQL Database तयार करा
```bash
mysql -u root -p < database/schema.sql
```
हे `featureflagdb` database आणि `feature_flags` table + 4 sample rows बनवेल.

### 2. DB credentials update करा
`src/main/java/com/featureflag/util/DBConnection.java` मध्ये तुमचा MySQL username/password टाका:
```java
private static final String USER = "root";
private static final String PASSWORD = "तुमचा_password";
```

### 3. Maven build करा (WAR file generate होईल)
```bash
mvn clean package
```
`target/FeatureFlagManager.war` तयार होईल.

### 4. Tomcat वर deploy करा
- Generated WAR file `<TOMCAT_HOME>/webapps/` folder मध्ये copy करा
- Tomcat start करा:
```bash
<TOMCAT_HOME>/bin/startup.sh    # Linux/Mac
<TOMCAT_HOME>/bin/startup.bat   # Windows
```

### 5. Browser मध्ये उघडा
```
http://localhost:8080/FeatureFlagManager/
```

**IDE मध्ये direct run करायचं असेल** (Eclipse/IntelliJ): project ला Maven project म्हणून import करा, 
Tomcat server configure करा, आणि "Run on Server" करा.

---

## ✨ Features
| Feature | Description |
|---|---|
| List all flags | Dashboard वर सगळे flags, त्यांचा status आणि environment दिसतो |
| Add flag | नवीन feature flag तयार करा (name, description, environment, enabled/disabled) |
| Edit flag | Existing flag चे details update करा |
| Toggle flag | एका click वर flag ON/OFF करा (code deploy न करता) |
| Delete flag | Flag कायमचा काढून टाका |

---

## 🔧 Possible Enhancements (पुढे वाढवायचं असेल तर)
- User authentication (login/roles: Admin vs Viewer)
- Audit log (कोणी, कधी flag बदलला)
- REST API endpoint (JSON) जेणेकरून इतर applications flags वाचू शकतील
- Environment-wise filtering on dashboard
- Search / pagination for मोठ्या flag lists साठी

---

## 📌 Note
- हा project **javax.servlet** API वापरतो (Tomcat 8.5 / 9.x). जर तुमच्याकडे Tomcat 10+ असेल, 
  तर सगळे `javax.servlet.*` imports `jakarta.servlet.*` मध्ये बदलावे लागतील.
- MySQL Connector/J driver आधीच `pom.xml` मध्ये dependency म्हणून add केलेला आहे, वेगळं jar लागत नाही.
