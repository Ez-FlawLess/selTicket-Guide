# راهنمای راه اندازی ویجت سلتیکت

## گرفتن Token و Application ID

## اطلاعات ارسالی برای ویجت

ویجت نیازمند گرفتن یک Object از کد شما به صورت زیر می باشد.

```typescript
interface SelTicketI {
    applicationId: string,
    token: string,
    onClose: () => void,
}
```
<ul>
  <li>
   <code>applicationId:</code> Your app ID. This is a static ID and is the same for all your users.
  </li>
  <li>
   <code>token:</code> for authenticating the user. You need to get this token for each user before loading the widget
  </li>
  <li>
   <code>onClose:</code> This function is called when ever the histoy stack of the widget is emtpy.
  </li>
</ul>

این Object را باید در window قرار دهید.

```typescript
export declare global {
    interface Window {
      selTicket?: SelTicketI,
      mountSelTicketWidget?: () => void,
    }
}
```

<ul>
  <li>
   <code>selTicket:</code> Config object.
  </li>
  <li>
   <code>mountSelTicketWidget:</code> Special case for SPA. expalined in example.
  </li>
</ul>


## دریافت فایل‌های برنامه

شما تنها نیاز به دریافت یک فایل JS برای ویجت هستید. لینک این فایل از طریق شرکت به شما داده می شود.

## راه اندازی
پس از انجام کانفیگ و دریافت فایل JS. ویجت در هر المنت که دارای آی دی selticket باشد اجرا می شود

### HTML

یک تگ ```script``` در کد قرار دهید و لینک دریافت شده از شرکت را در ```src``` قرار دهید. قبل از این تگ باید ```window.selTicket``` با اطلاعات مورد نیاز را ساخته باشید.

```html
<div id="selticket" />

<script>
    window.selTicket = {
        applicationId: APPLICATION_ID,
        token: TOKEN,
        onClose: () => window.history.back(),
    }
</script>
<script type="text/javascript" src="SCRIPT_LINK" />
```

### React

برای React 2 روش وجود دارد.

#### روش اول

شما می توانید در index.html تگ اسکریپت را برای ویچت قرار دهید و زمانی که می خواستید ویجت رندر شود. کانفیگ را پر کنید و سپس دستور زیر را اجرا کنید.

```typescript
window.mountSelTicketWidget()
```

این روش به دلیل آن که فایل های ویجت حتی اگر کاربر به صفحه‌ی ویجت نرود هم لود می شوند. ایده آل نیست.

#### روش دوم

در useEffect صفحه ای که ویجت لود می شود. اسکریپت را اضافه کنید.

```typescript
useEffect(() => {
    window.selTicket = {
        applicationId: APPLICATION_ID,
        token: TOKEN,
        onClose: () => {
            // Handle going back with router
        },
    }

    const script = document.createElement('script');
    script.id = "selticket-script"
    script.type = 'text/javascript';
    script.src = 'SCRIPT_LINK';    

    if (!document.head.contains(script)) document.head.appendChild(script);
    else window.mountSelTicketWidget()
}, [])

return (
    <div id="selticket" />
)
```

### شخصی سازی

شما برای تغییر رنگ‌های استفاده شده در ویجت می توانید variable هر کدام را در css خود به شکل زیر تغییر دهید.

```css
#selticket {
  --selticket-primary-color: ******;
  --selticket-primary-color-container: ******;;
  --selticket-secondary-color: ******;;
  --selticket-secondary-color-container: ******;;
  --selticket-accent-color: ******;
  --selticket-accent-color-container: ******;
  --selticket-on-primary-color: ******;
  --selticket-on-primary-color-container: ******;
  --selticket-on-secondary-color: ******;
}
```

اگر مایل به استفاده از رنگ های پیش فرض هستید. css variable آن را در فایل css خود قرار ندهید.
