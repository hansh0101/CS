# Intent

<aside>
๐ก Intent์ ๋ํด ์์๋ณด์.

</aside>

์ธํํธ๋ messaging object(๋ฉ์์ง ๊ฐ์ฒด)๋ค. ์ด ๊ฐ์ฒด๋ฅผ ํตํด ๋ค๋ฅธ ์ปดํฌ๋ํธ ๊ฐ์ ์ ๋ณด๋ฅผ ์ฃผ๊ณ ๋ฐ์ ์ ์๋ค.

### Intent Types

1. **`Explicit Intents`**
    
    ๋ช์์  ์ธํํธ๋ ์ธํํธ์ ํด๋์ค ๊ฐ์ฒด๋ ์ปดํฌ๋ํธ ์ด๋ฆ์ ์ง์ ํ์ฌ ํธ์ถํ  ๋์์ ํ์คํ ์ ์ ์๋ ๊ฒฝ์ฐ์ ์ฌ์ฉํ๋ค. (์ฃผ๋ก ์ ํ๋ฆฌ์ผ์ด์ ๋ด๋ถ์์ ์ฌ์ฉ)
    
2. **`Implicit Intents`**
    
    ์์์  ์ธํํธ๋ ํธ์ถํ  ๋์์ด ๋ฌ๋ผ์ง ์ ์๋ ๊ฒฝ์ฐ์ ์ฌ์ฉํ๋ค. (์๋๋ก์ด๋ ์์คํ์ด ์ธํํธ๋ฅผ ์ด์ฉํด ์์ฒญํ ์ ๋ณด๋ฅผ ์ฒ๋ฆฌํ  ์ ์๋ ์ ์ ํ ์ปดํฌ๋ํธ๋ฅผ ์ฐพ์ ์ฌ์ฉ์์๊ฒ ๊ทธ ๋์๊ณผ ์ฒ๋ฆฌ ๊ฒฐ๊ณผ๋ฅผ ๋ณด์ฌ์ค๋ค.)
    

![Untitled](Intent%206ad32/Untitled.png)

### IntentFilter

์์์  ์ธํํธ๋ฅผ ํตํด ์ฌ์ฉ์๋ก ํ์ฌ๊ธ ์ด๋ ์ฑ์ ์ฌ์ฉํ ์ง ์ ํํ๋๋ก ํ  ๋ IntentFilter๊ฐ ํ์ํ๋ค.

### PendingIntent

Intent๋ฅผ ๊ฐ์ง๊ณ  ์๋ ํด๋์ค๋ก, ๊ธฐ๋ณธ ๋ชฉ์ ์ ๋ค๋ฅธ ์ ํ๋ฆฌ์ผ์ด์(๋ค๋ฅธ ํ๋ก์ธ์ค)์ ๊ถํ์ ํ๊ฐํ์ฌ ๊ฐ์ง๊ณ  ์๋ Intent๋ฅผ ๋ง์น ๋ณธ์ธ ์ฑ์ ํ๋ก์ธ์ค์์ ์คํํ๋ ๊ฒ์ฒ๋ผ ์ฌ์ฉํ๋ ๊ฒ์ด๋ค.

Notification์ ์๋๋ก์ด๋ ์์คํ์ NotificationManager๊ฐ Intent๋ฅผ ์คํํ๋ค. ์ฆ ๋ค๋ฅธ ํ๋ก์ธ์ค์์ ์ํํ๊ธฐ ๋๋ฌธ์ Notification์ผ๋ก Intent ์ํ ์ PendingIntent์ ์ฌ์ฉ์ด ํ์์ ์ด๋ค.