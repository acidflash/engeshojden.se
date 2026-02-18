# DNS-konfiguration för engeshojden.se

## Översikt
För att få din domän `engeshojden.se` att fungera med GitHub Pages behöver du konfigurera DNS-poster hos din domänleverantör.

## Steg-för-steg instruktioner

### 1. Aktivera GitHub Pages
Först behöver du aktivera GitHub Pages för ditt repository:

1. Gå till repository-inställningar på GitHub
2. Navigera till "Pages" under "Code and automation"
3. Under "Source", välj den branch du vill använda (t.ex. `main` eller `copilot/create-static-website`)
4. Klicka på "Save"

### 2. Konfigurera DNS-poster

Du behöver lägga till följande DNS-poster hos din domänleverantör (t.ex. Loopia, One.com, GoDaddy, etc.):

#### Alternativ A: Använd apex-domän (engeshojden.se)

Lägg till följande **A-poster** som pekar till GitHubs IP-adresser:

```
Typ: A
Namn: @ (eller lämna tomt för root-domän)
Värde: 185.199.108.153
TTL: 3600 (eller standardvärde)

Typ: A
Namn: @ (eller lämna tomt för root-domän)
Värde: 185.199.109.153
TTL: 3600

Typ: A
Namn: @ (eller lämna tomt för root-domän)
Värde: 185.199.110.153
TTL: 3600

Typ: A
Namn: @ (eller lämna tomt för root-domän)
Värde: 185.199.111.153
TTL: 3600
```

**OCH** lägg till en **AAAA-post** för IPv6 (valfritt men rekommenderat):

```
Typ: AAAA
Namn: @ 
Värde: 2606:50c0:8000::153
TTL: 3600

Typ: AAAA
Namn: @
Värde: 2606:50c0:8001::153
TTL: 3600

Typ: AAAA
Namn: @
Värde: 2606:50c0:8002::153
TTL: 3600

Typ: AAAA
Namn: @
Värde: 2606:50c0:8003::153
TTL: 3600
```

#### Alternativ B: Använd www-subdomain (www.engeshojden.se)

Lägg till en **CNAME-post**:

```
Typ: CNAME
Namn: www
Värde: acidflash.github.io
TTL: 3600
```

#### Rekommendation: Konfigurera både apex och www

För bästa användarupplevelse, konfigurera både apex-domänen (engeshojden.se) och www-subdomänen:

1. Lägg till alla A-poster (och AAAA-poster) för apex-domänen enligt Alternativ A
2. Lägg till CNAME-posten för www enligt Alternativ B

### 3. Konfigurera Custom Domain i GitHub

Efter att DNS-posterna är tillagda:

1. Gå tillbaka till GitHub Pages-inställningarna
2. Under "Custom domain", skriv in `engeshojden.se`
3. Klicka på "Save"
4. Vänta på att DNS-kontrollen går igenom (kan ta några minuter)
5. När kontrollen är klar, aktivera "Enforce HTTPS" för säker anslutning

### 4. Lägg till CNAME-fil i repository

GitHub Pages kräver en CNAME-fil i root-katalogen för att bevara din custom domain-konfiguration:

Skapa en fil `CNAME` med följande innehåll:
```
engeshojden.se
```

### 5. Verifiera konfigurationen

Efter att DNS-posterna har propagerats (kan ta upp till 24-48 timmar, men oftast inom några minuter till timmar):

**Kontrollera DNS-poster:**
```bash
# Kontrollera A-poster
dig engeshojden.se A

# Kontrollera CNAME
dig www.engeshojden.se CNAME
```

**Testa i webbläsare:**
- Öppna `https://engeshojden.se` i din webbläsare
- Öppna `https://www.engeshojden.se` i din webbläsare

Båda bör visa din sida med "Engeshöjden" på mörk bakgrund.

## Viktiga noteringar

- **DNS-propagering**: Det kan ta upp till 24-48 timmar för DNS-ändringar att spridas globalt, men oftast går det mycket snabbare (minuter till timmar)
- **HTTPS**: GitHub Pages tillhandahåller automatiskt gratis SSL/TLS-certifikat via Let's Encrypt
- **TTL**: Time To Live (3600 sekunder = 1 timme) bestämmer hur länge DNS-caches behåller informationen

## Felsökning

### Problem: Sidan visar inte
- Kontrollera att DNS-posterna är korrekt konfigurerade
- Vänta på DNS-propagering
- Kontrollera att GitHub Pages är aktiverat i repository-inställningarna

### Problem: HTTPS fungerar inte
- Vänta några minuter efter att custom domain är konfigurerad
- GitHub genererar automatiskt SSL-certifikatet, men det kan ta lite tid

### Problem: "Domain's DNS record could not be retrieved"
- Kontrollera att A-posterna är korrekt konfigurerade
- Vänta på DNS-propagering
- Kontrollera med din domänleverantör att ändringarna har sparats

## Resurser

- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [Managing a custom domain for GitHub Pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)
- [DNS Checker Tool](https://dnschecker.org/) - för att kontrollera DNS-propagering globalt
