# စာမူကြမ်း

ဘယ်က စရေးမလဲ?

စိတ်ထဲမှာ ရှိတာ အရင်ချရေးမယ်။

## API

### API တွေက RESTFUL ဆန်ရမယ်။

GET /api/v1/books (Nourn ကိုမှ Plural ဖြစ်သင့်)
POST /api/v1/books 
PUT /api/v1/books/{bookId}
PATCH /api/v1/books/{bookId}
DELETE /api/v1/books/{bookId}

### Request Response format တွေ consistency ရှိဖို့

Request

single entity 

```json
{}
```

mulitple payload

```json
[
    {},
    {}
]
```

အဓိကကတော့ Response တွေပဲ

```json
{
    "status" : true / false,
    "message" : "Human Readable Text Message",
    "data" : {} , [{}],
    "meta" : {
        "pagination" : {
            "current_page" : 1,
            "limit" : 20,
            "total" : 120,
            "pages" : 6
        }
    },
    "errors" : [
        {
            "field" : "field_name", // field name က nested တွေ ဆိုရင် person.spouse.name ဒါမျိုး လာမယ်။ တကယ်လို့ array တွေ ဆိုရင်လည်း person.spouses[0].name ဒါမျိုး လာမယ်။
            "code" : "VALIDATION_FAILURE",
            "message" : "the field_name is required"
        }
    ]
}
```

status code ကိုလည်း သင့်တင့်အောင် ပြန်ပေးသင့်တယ်။

200	အောင်မြင်ပြီး data ပြန်ပေး
201	အသစ်ဆောက်ပြီး (Location header ပါရမယ်)
204	အောင်မြင်ပြီး body မရှိ (DELETE)
400	Request ပုံစံမှား
401	Login မလုပ်ရသေး / token မမှန်
403	Login ဝင်ထားပေမယ့် ခွင့်ပြုချက်မရှိ
404	ရှာမတွေ့
409	Conflict (ဥပမာ email ထပ်နေ)
422	Validation မအောင်
429	Request များလွန်း
500	Server အမှား

တက်နိုင်သမျှတော့ 500 မပြန်အောင် လုပ်ရမယ်။
ပြန်ခဲ့ရင်လည်း server log မှာ ချက်ခြင်း ဝင်ဖတ်နိုင်အောင်, developer ကို notify လုပ်နိုင်အောင် စီမံထားရမယ်။

ဘာပဲ ပြန်ပြန် အပေါ်က format တော့ ဝင်အောင်ပြန်ပေးရမယ်။

### Business Endpoints

CRUD ထက်ပိုလာတဲ့ endpoint တွေကိုတော့ အဓိပ္ပါယ်ရှိတာမျိုး ပေးကြမယ်။
ဉပမာ။

GET api/v1/dashboard
GET api/v1/book-sale-reports

ဒေတာပြန်တာကတော စံသတ်မှတ်ထားတဲ့ ပုံစံအတိုင်း ပြန်ကြမယ်။

## Entity တွေ အကြောင်း

Entity တွေကလည်း naming convention တစ်ခုနဲ့ သတ်သတ်မှတ်မှတ်လေး ပေးတာကောင်းမယ်။

snake_case ပဲဆိုကြပါစို့။

ကြားထဲမှာ တစ်ခု ဖြတ်ပြောဖို့ လိုတာက ID

integer ထက် uuid (၁၆) လုံးပဲ default အနေနဲ့ သုံးသင့်တယ်။
integer ID ကို default သုံးတဲ့ laravel, mysql , mariadb ဆိုရင်လည်း တစ်ခုခုကြံပေါ့။
Book

```json
{
    "id" : "UUID",
    "name" : "String",
    "description" : "String",
    "category_id" : "UUID",
    "category" : {
        "id" : "UUID",
        "name" : "String",
        "description" : "String",
        "created_at" : "DATETIME WITH UTC / MM",
        "updated_at" : "DATETIME WITH UTC / MM"
    },
    "created_at" : "DATETIME WITH UTC / MM",
    "updated_at" : "DATETIME WITH UTC / MM"
}
```

