# FastAPI Full Course (Hindi) — Part 2 Notes (Roman Urdu)

**Video:** [FastAPI Full Course 2026 in Hindi — Mohit Decodes](https://www.youtube.com/watch?v=fxRCoEUmq8s&t=294s)
**Topics:** `00:04:54 → 00:40:00` (Roadmap, Install, Python version, Global install, Hello World, Virtual Environment, Swagger UI, Routes & GET, Multiple Routes, Path Params, Query Params, Optional Params, Default Values)
**Language:** Roman Urdu (aasan notes — kya sikhaya + poora code)

---

## 00:04:54 — Course Roadmap & Requirements

**Kya sikhaya:**

- Course ka roadmap ye hai (isi order me sab aayega):
  1. Basics se start (install + setup)
  2. Routes aur GET APIs banana
  3. **CRUD APIs** (Create, Read, Update, Delete)
  4. Database connect karna
  5. **JWT se Authentication**
  6. File upload karna
  7. Deployment karna
  8. End me **production-level project**
- **Requirement:** sirf **basic Python** aana chahiye. Zyada requirements nahi hain.
- Editor ke liye **VS Code** use karna hai.
- Ye course sirf syntax nahi sikhata — real companies me APIs kaise banti hain aur deploy hoti hain wo bhi sikhata hai.

---

## 00:05:41 — Installing FastAPI & Setting Up Virtual Environment

**Kya sikhaya:**

- Is lecture me pehle check karenge ke Python install hai ya nahi, phir **FastAPI + Uvicorn** install karenge, phir **virtual environment** setup karenge.
- **Do tareeqe** sikhaye jayenge:
  1. **Without venv (global install)** — quick tareeqa, teaching ke liye easy
  2. **With venv** — recommended tareeqa (industry me 99.9% projects venv me hi bante hain)
- Instructor ne clear kaha: *"Main tumhe dono tareeqe sikhata hoon, lekin tumhe practice venv ke andar hi karni hai."*
- Kyun? Kyunki professional machine pe multiple projects hote hain — har project ka Python version aur FastAPI version alag hota hai. Venv na ho to versions clash karte hain aur dependencies mix ho jati hain.
- Aage project structure aisa hoga:

```text
project/
├── main.py          # main application
├── static/          # CSS / JS files
├── templates/       # HTML files
├── database/        # DB related files
├── logger/          # logging
└── env/  (or venv/) # virtual environment
```

---

## 00:07:31 — Checking Python Version

**Kya sikhaya:** Python system me hai ya nahi, aur version kya hai — ye check karna.

**Commands:**

```bash
# Windows
python --version

# Mac / Linux
python3 --version
```

**Important points:**

- **Mac pe `python` command kaam nahi karti** — Apple ne internal configuration me `python` already kisi cheez ke liye set kar rakha hai, is liye Mac users `python3 --version` likhein.
- Python ka version **3.8 ya us se upar** hona chahiye — tabhi FastAPI theek se kaam karega.
- Agar error aa raha hai to Python dobara install karna hoga (instructor ne apni Python playlist me ye sikha diya hua hai).
- Pehle basic Python seekho, phir FastAPI — sequence yehi hai.

---

## 00:09:28 — Global Installation of FastAPI & Uvicorn

**Kya sikhaya:** Bina virtual environment ke (globally) install karna — quick tareeqa.

**Commands:**

```bash
pip install fastapi
pip install uvicorn
```

- `fastapi` → framework (API banane ke liye)
- `uvicorn` → **server** jo humari application ko run karta hai
- Check karne ke liye:

```bash
pip list
```

Isme `fastapi` aur `uvicorn` dono nazar aa jayenge.

**Warning (video me clear kiya gaya):**

- Global install ka matlab hai **poore system me install** — ye production ya multiple projects ke liye **recommended nahi**.
- Problem: different projects ki dependencies aur versions **clash** kar jati hain.
- Isi liye industry me **venv bana kar** projects bante hain.

---

## 00:10:56 — First "Hello World" API

**Kya sikhaya:** Pehli chhoti si FastAPI app banana aur usko server pe run karna.

**Code — `main.py`:**

```python
from fastapi import FastAPI

app = FastAPI()          # FastAPI ka object bana liya

@app.get("/")            # home route
def home():
    return {"message": "Hello World - Without Virtual Environment"}
```

**Run karne ka command (yaad rakho):**

```bash
uvicorn main:app --reload
```

- `main` → file ka naam (`main.py`)
- `app` → andar jo FastAPI object banaya hai us variable ka naam
- `--reload` → code change karne pe server khud restart ho jata hai (baar baar khud restart nahi karna padta)

**Output:**

- Server run hone ke baad browser me URL kholein → JSON me data milta hai:

```json
{ "message": "Hello World - Without Virtual Environment" }
```

- Yaad rakho: browser me jo dikh raha hai wo **page nahi**, ye sirf **API ka JSON data** hai — isi ko frontend consume karke website pe render karega.

---

## 00:13:13 — Creating Virtual Environment (venv)

**Kya sikhaya:** Project ke liye alag environment banana (dependency isolation).

**Kyun zaroori hai:**

- Har project ka **Python version alag** ho sakta hai, **FastAPI version alag** ho sakta hai.
- Venv se har project ki dependencies alag rehti hain — kuch clash nahi hota.
- Technically bhi yehi **recommended** tareeqa hai.

**1. Venv banao:**

```bash
# Windows
python -m venv env

# Mac / Linux
python3 -m venv env
```

(naam kuch bhi rakh sakte ho — `env`, `venv`, kuch bhi. Instructor ne `env` rakha tha.)

**2. Activate karo:**

```bash
# Windows
env\Scripts\activate

# Mac / Linux
source env/bin/activate
```

- Activate hone ke baad terminal ke start me **`(env)`** nazar aata hai → samajh jao venv activate ho gaya.

**3. Ab isi venv ke andar install karo:**

```bash
pip install fastapi uvicorn
pip list        # check karne ke liye
```

- `pip list` me sirf wahi packages milenge jo tumne venv me install kiye (Django waghera nahi) — yahi **isolation** ka proof hai.

**4. Phir `main.py` banao aur run karo:**

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def home():
    return {"message": "Hello World from FastAPI (venv)"}
```

```bash
uvicorn main:app --reload
```

- Dono tareeqon (venv aur without venv) ka result same hai — bas commands aur isolation ka farak hai.
- **Practice hamesha venv ke andar karo.**

---

## 00:17:11 — Automatic API Documentation (Swagger UI)

**Kya sikhaya:** FastAPI apne aap jo documentation banata hai wo kaise dekhein aur use karein.

**Kaise kholo:** Server chalne ke baad URL ke aage `/docs` laga do:

```text
http://127.0.0.1:8000/docs
```

**Swagger UI me kya milta hai:**

- Tumhari saari APIs (routes) ki list — naam, method (GET/POST), description ke saath
- **"Try it out"** button → input fields → **"Execute"**
- Execute karne pe:
  - **Requested URL** (jo request gayi)
  - **Response body** (JSON data)
  - **Status code: 200** (success)
  - Content-Length, Content-Type (`application/json`)
  - **Schema** (jaise "string") — response ka type bhi dikha deta hai

**Sabse bari baat:**

- **Postman ki zaroorat nahi** — API testing sab kuch yahin Swagger UI me ho jati hai.
- Ye docs **automatically generate** hoti hain — code likho, docs khud ban jati hain.

---

## 00:18:23 — API Routes & GET Requests

**Kya sikhaya:** Route hota kya hai aur GET request ka matlab kya hai.

**Route kya hai:**

- **Route = URL path** jahan pe specific function execute hota hai.
- `/` → home route
- `/about` → about page ka route
- `/users` → users ka data wala route
- **Har route ke saath ek function attach hota hai** — jab route hit hota hai, wahi function chal jata hai.

**GET request kya hai:**

- GET ka matlab hai **data fetch karna (laana)** — data ko **change nahi** karte, sirf **read** karte ho.
- Yaad rakhne ka simple tareeqa: **"GET aaya to sirf data fetch ki baat hai."**
- Real examples:
  - **Google** pe search karo → GET request se data
  - **Instagram** ki feed / reels → GET API se
  - **Amazon** pe products dekhna → GET API se backend se aata hai

**Code:**

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")                       # decorator: is route ka method GET hai
def home():
    return {"message": "Welcome to FastAPI"}
```

**Extra baat:**

- Browser me ye JSON data hi dikhega — ye page nahi hai. Frontend isi API ko call karke HTML me display karega.
- Testing: browser me URL kholo, ya Swagger UI me "Try it out" karo, ya Postman use karo (zaroori nahi).

---

## 00:22:45 — Creating Multiple Routes

**Kya sikhaya:** Ek hi app me multiple routes banana — sabka apna path aur apna function.

**Code:**

```python
from fastapi import FastAPI

app = FastAPI()

# Home Route
@app.get("/")
def home():
    return {"message": "Welcome to FastAPI"}

# About Route
@app.get("/about")
def about():
    return {"message": "This is about page"}

# Users Route (JSON me list bhi bhej sakte ho)
@app.get("/users")
def users():
    return {"users": ["Mohit", "Rohit", "Amit"]}
```

**Run karo:**

```bash
uvicorn main:app --reload
```

**Browser me:**

- `/` → `{"message": "Welcome to FastAPI"}`
- `/about` → `{"message": "This is about page"}`
- `/users` → `{"users": ["Mohit", "Rohit", "Amit"]}` ← JSON me multiple data bhi easily bhej sakte ho

**Points:**

- `--reload` laga hua hai to code save karne pe server khud restart ho jata hai — baar baar reload nahi karna padta.
- Swagger UI (`/docs`) me teeno routes aayenge aur unhe execute bhi kar sakte ho.
- Browser me jo dikhta hai wo **API ka JSON hai, webpage nahi** — isi data ko frontend apni website me use karta hai.

**Is lecture ke Interview Questions (video me bataye):**

| Sawal | Jawab |
|---|---|
| API kya hoti hai? | Ek **bridge** jo frontend aur backend ko connect karta hai. Frontend request bhejta hai, backend response deta hai (mostly JSON) |
| Route kya hota hai? | Ek **URL path** jahan specific function execute hota hai (path ke basis pe) |
| GET request kya hai? | Server se **data fetch** karne ke liye — data change nahi hota, sirf read hota hai |
| FastAPI JSON kyun return karta hai by default? | Kyunki API ka main purpose **data exchange** hai, aur JSON **lightweight + easy to use** format hai frontend ke liye |
| Browser me API kaise hit karte ho? | URL browser me daalo → us URL ke hisaab se API directly test ho jati hai |
| Swagger UI kya hai? | Ek **auto-generated API documentation tool** jisme **bina Postman** ke API test kar sakte ho |

---

## 00:27:26 — Dynamic Routes & Path Parameters

**Kya sikhaya:** Aise routes jinka hissa **dynamically change** hota hai — path parameters ke through.

**Path Parameter kya hai:**

- URL ke andar jo **dynamic value** change hoti hai — usko **path parameter** kehte hain.
- Example: `/users/1`, `/users/2`, `/users/100` → **route ek hi**, lekin output har ID ke hisaab se **change** hota hai.
- Real life example: **E-commerce** me product pe click karo → product ki ID ke base pe uski poori detail khul jati hai. Route wahi rehta hai, bas ID change hoti hai.

**Code:**

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/users/{user_id}")        # {} = dynamic hissa
def get_user(user_id):              # jo ID URL se aayi, wo yahan parameter me mil gayi
    return {"user_id": user_id}
```

**Test:**

- `/users/1001` → `{"user_id": "1001"}`
- `/users/200` → `{"user_id": "200"}` ← ID change ki, output change ho gaya. Yahi **dynamic routing** hai.
- `/users` (bina ID) → `{"detail": "Not Found"}` — kyunki ID di hi nahi.

**Type Validation (FastAPI ka automatic validation):**

```python
@app.get("/users/{user_id}")
def get_user(user_id: int):        # int type fix kar diya
    return {"user_id": user_id}
```

- `/users/123` → sahi chalega
- `/users/abc` → **automatic error**:
  > `Input should be a valid integer, unable to parse string as an integer`

- Matlab: **FastAPI khud se data validation karta hai** — tumhe manually check nahi karna padta (yehi uski bari khoobi hai).
- Type `str` kar do to `abc` bhi accept ho jayega.
- Swagger UI me bhi validation test kar sakte ho — galat value pe wahi integer wala error aayega.

**Is lecture ke Interview Questions:**

| Sawal | Jawab |
|---|---|
| Path parameter kya hota hai? | URL ke andar ki dynamic value, jisse route ka output change hota hai |
| Path params vs Query params me farak? | Path params URL path ka hissa (`/users/101`), query params `?` ke baad aate hain (`/users?name=ali`) — detail next lecture |
| Type validation kaise hota hai? | FastAPI function parameter ke type hint (`user_id: int`) se khud validate karta hai; galat type pe 422 validation error |
| Path parameter ka real use case? | Database se **specific record fetch** karna — user by ID, product by ID |

---

## 00:32:58 — Query Parameters & Optional Parameters

**Kya sikhaya:** URL ke end me `?` ke baad jo extra data aata hai — usko kaise handle karein, aur usko **optional** kaise banayein.

**Query Params kya hain:**

- URL ke **end me `?` ke baad** jo extra data hota hai:

```text
/users?name=mohit
/products?price=1000
/category?sort=latest
```

- `?` ke baad jo key hai (jaise `name`), uske **base pe value handle** hoti hai — isi ko **query parameter** kehte hain.
- **Kahan use hote hain:** filtering, searching, sorting
- **Real examples:**
  - **Amazon** pe price/category ka filter lagana → query params
  - **Instagram** pe user search karna → query params

**Code (required query param):**

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/users")
def get_users(name: str):           # name required hai
    return {"name is equal to": name}
```

- `/users?name=mohit` → `{"name is equal to": "mohit"}` ✅
- Sirf `/users` (bina name) → **automatic error**: `field required`, input null → kyunki FastAPI ne khud validation kar di.

**Optional Parameter kaise banayein:**

- Query params me **by default optional** rakhna behtar hai, warna jab user parameter na de to error aayega.
- Solution: default value **`None`** de do:

```python
@app.get("/users")
def get_users(name: str = None):    # ab optional hai
    return {"name is equal to": name}
```

- `/users` → ab error nahi aayega (kuch nahi milega / null dikhega)
- `/users?name=mohit` → `{"name is equal to": "mohit"}` ✅

---

## 00:36:58 — Default Values in Query Parameters

**Kya sikhaya:** Query parameter ki **default value** kaise set karte hain, aur **multiple query params** ek saath kaise handle karte hain.

**1. Default Value set karna:**

```python
@app.get("/products")
def get_products(limit: int = 10):   # default limit = 10
    return {"limit": limit}
```

- `/products` → `{"limit": 10}` ← user ne kuch diya hi nahi, to **default value** chali gayi
- `/products?limit=200` → `{"limit": 200}` ← jo user ne di, wohi mil gayi
- Matlab: agar user value dena bhool jaye, to default value use ho jati hai.

**2. Multiple Query Params handle karna:**

```python
@app.get("/items")
def get_items(name: str = None, price: int = 0):   # ek optional + ek default
    return {"name": name, "price": price}
```

- `/items` → `{"name": null, "price": 0}` (defaults)
- `/items?name=laptop&price=2000` → `{"name": "laptop", "price": 2000}` ← dono values mil gayi
- Ek hi API me **multiple filters** laga sakte ho — real projects me bilkul aise hi hota hai.

**Swagger UI se practice:**

- `/docs` me ye route kholo → **"Try it out"**
- Yahan FastAPI pehle se bata deta hai: kaunsa param **integer** hai, kaunsa **string**, aur **default value** kya hai (jaise price = 0)
- Values daalo → **Execute** → respone body mil jayega; **Requested URL** bhi automatically update ho jata hai (aise samajh aata hai query params kaise lagte hain)

**Is lecture ke Interview Questions (video me bataye):**

| Sawal | Jawab |
|---|---|
| Query params kya hote hain? | URL ke `?` ke baad wala extra data, jisse filtering/searching hoti hai |
| Query vs Path params ka farak? | Path param URL ka hissa hota hai (`/users/101`), query param `?key=value` ke form me (`/users?name=ali`) |
| Query param ko optional kaise banate hain? | Default value de kar — `name: str = None` |
| Default value kaise set karte hain? | Parameter ke saath value likh kar — `limit: int = 10` |
| Multiple query params kaise handle karte hain? | Function me comma laga kar multiple parameters likho — `name: str = None, price: int = 0` |
| Query params ke real world use cases? | **Search, filter, sorting aur pagination** |

---

## Quick Cheat Sheet (Commands — yaad rakho)

```bash
# Python check
python --version            # Windows
python3 --version           # Mac/Linux   (version 3.8+ hona chahiye)

# Global install (quick, recommended nahi)
pip install fastapi
pip install uvicorn
pip list

# Virtual environment
python -m venv env                 # venv banao (Mac: python3 -m venv env)
env\Scripts\activate               # Windows activate
source env/bin/activate            # Mac/Linux activate
pip install fastapi uvicorn        # venv ke andar install
pip list                           # sirf venv ke packages dikhenge

# Server run karna (sabse important command)
uvicorn main:app --reload
#   main  = file name (main.py)
#   app   = FastAPI() object ka naam
#   --reload = auto restart on code change

# Browser me
http://127.0.0.1:8000          # API ka JSON response
http://127.0.0.1:8000/docs     # Swagger UI (auto API docs + testing)
```

## Ek Nazar Me — Concepts (30-Second Recap)

- **Roadmap:** basics → CRUD → DB → JWT auth → file upload → deployment → production project. Requirement: sirf basic Python + Python 3.8+.
- **Install:** FastAPI framework + Uvicorn server. Global install quick hai lekin versions clash karte hain → **hamesha venv** use karo.
- **Venv:** `python -m venv env` → activate → `pip install fastapi uvicorn` → `(env)` prompt me dikhta hai.
- **Run:** `uvicorn main:app --reload` → browser me JSON output.
- **Swagger UI:** `/docs` pe auto-generated docs — bina Postman API testing.
- **Route:** URL path + attached function. `/`, `/about`, `/users` jaise multiple routes ban sakte hain.
- **GET:** sirf data **fetch** karta hai, change nahi karta (Google search, Instagram feed, Amazon products).
- **Path param:** `/users/{user_id}` — URL ke andar dynamic value; `user_id: int` likho to **automatic type validation** bhi ho jati hai.
- **Query param:** `?key=value` — searching/filtering/sorting ke liye; required ho to error, `= None` karo to **optional**, `= 10` jaisa default do to **default value**.
- **Multiple query params:** `name: str = None, price: int = 0` — ek hi API me multiple filters.

---

**Next topic (agla lecture):** `00:41:05 — Request Body & POST Requests` (data bhejna seekhenge, POST request aur Pydantic ka intro).
