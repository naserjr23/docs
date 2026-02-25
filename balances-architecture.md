# نظام الأرصدة - الهندسة المعمارية

## نظرة عامة
رسم بياني يوضح الهندسة المعمارية الكاملة لنظام الأرصدة والمحافظ.

```mermaid
graph TB
    subgraph Client["العملاء"]
        Web["تطبيق ويب"]
        Mobile["تطبيق موبايل"]
        Dashboard["لوحة التحكم"]
    end
    
    subgraph API["طبقة APIs"]
        BalanceAPI["API الأرصدة"]
        TransactionAPI["API العمليات"]
        WalletAPI["API المحافظ"]
    end
    
    subgraph Business["طبقة الأعمال"]
        BalanceService["خدمة الأرصدة"]
        TransactionService["خدمة العمليات"]
        ValidationService["خدمة التحقق"]
    end
    
    subgraph Data["طبقة البيانات"]
        BalanceDB[(قاعدة بيانات الأرصدة)]
        TransactionDB[(قاعدة بيانات العمليات)]
        Cache["ذاكرة التخزين المؤقتة"]
    end
    
    subgraph Integration["التكامل الخارجي"]
        PaymentGateway["بوابة الدفع"]
        NotificationService["خدمة الإخطارات"]
        AuditLog["سجل التدقيق"]
    end
    
    Web -->|HTTP/REST| BalanceAPI
    Mobile -->|HTTP/REST| BalanceAPI
    Dashboard -->|HTTP/REST| BalanceAPI
    
    BalanceAPI --> BalanceService
    TransactionAPI --> TransactionService
    WalletAPI --> BalanceService
    
    BalanceService --> ValidationService
    TransactionService --> ValidationService
    
    BalanceService --> Cache
    BalanceService --> BalanceDB
    TransactionService --> TransactionDB
    
    BalanceService --> PaymentGateway
    TransactionService --> NotificationService
    BalanceService --> AuditLog
    
    Cache -.->|تحديث| BalanceDB
```

## مكونات النظام

### 1. طبقة العملاء (Client Layer)
- **تطبيق ويب**: واجهة المستخدم الرئيسية للويب
- **تطبيق موبايل**: تطبيق الهاتف الذكي
- **لوحة التحكم**: لوحة إدارة النظام

### 2. طبقة واجهات البرمجة (API Layer)
- **API الأرصدة**: التعامل مع عمليات الأرصدة
- **API العمليات**: إدارة العمليات المالية
- **API المحافظ**: إدارة المحافظ الرقمية

### 3. طبقة الأعمال (Business Logic Layer)
- **خدمة الأرصدة**: معالجة منطق الأرصدة
- **خدمة العمليات**: معالجة العمليات المالية
- **خدمة التحقق**: التحقق من صحة البيانات والعمليات

### 4. طبقة البيانات (Data Layer)
- **قاعدة بيانات الأرصدة**: تخزين بيانات الأرصدة
- **قاعدة بيانات العمليات**: تخزين سجل العمليات
- **ذاكرة التخزين المؤقتة**: تحسين الأداء

### 5. التكامل الخارجي (External Integration)
- **بوابة الدفع**: معالجة المدفوعات
- **خدمة الإخطارات**: إرسال التنبيهات والرسائل
- **سجل التدقيق**: تسجيل جميع العمليات

## تدفق العمليات

1. يرسل العميل طلب عبر واجهة برمجية
2. يتم التحقق من صحة الطلب في خدمة التحقق
3. تعالج خدمة الأرصدة الطلب
4. يتم تحديث قاعدة البيانات وذاكرة التخزين المؤقتة
5. يتم إرسال إخطار للعميل
6. يتم تسجيل العملية في سجل التدقيق