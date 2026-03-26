# Deploy Privado — MEDEX Sprint Review

## Opção 1: Netlify (Recomendado — mais rápido)

### Passo 1: Instalar Netlify CLI
```bash
npm install -g netlify-cli
```

### Passo 2: Criar pasta de deploy
```bash
mkdir medex-sprint && cp index.html medex-sprint/
```

### Passo 3: Deploy
```bash
cd medex-sprint
netlify deploy --prod --dir=.
```
> Na primeira vez, vai pedir para linkar/criar um site. Siga o wizard.

### Passo 4: Adicionar autenticação (password protection)
1. Crie um arquivo `_headers` na mesma pasta:
```
/*
  Basic-Auth: medex:sprint2026
```
2. Ou vá em **Site settings → Access control → Password protection** no dashboard Netlify
3. Defina password: `sprint2026`
4. Redeploy: `netlify deploy --prod --dir=.`

**Resultado:** URL tipo `https://medex-sprint-review.netlify.app` — protegida por senha.

---

## Opção 2: Vercel

### Passo 1: Instalar Vercel CLI
```bash
npm install -g vercel
```

### Passo 2: Criar pasta de deploy
```bash
mkdir medex-sprint && cp index.html medex-sprint/
```

### Passo 3: Criar `vercel.json` na pasta
```json
{
  "version": 2,
  "builds": [{ "src": "index.html", "use": "@vercel/static" }],
  "routes": [{ "src": "/(.*)", "dest": "/index.html" }]
}
```

### Passo 4: Deploy
```bash
cd medex-sprint
vercel --prod
```

### Passo 5: Password Protection
Vercel Pro/Enterprise tem password protection nativo:
1. Dashboard → Project → Settings → General → Password Protection
2. Enable e defina: `sprint2026`

**Alternativa gratuita:** Use Vercel Edge Middleware para basic auth:

Crie `middleware.js`:
```javascript
export default function middleware(req) {
  const auth = req.headers.get('authorization');
  if (!auth || auth !== 'Basic ' + btoa('medex:sprint2026')) {
    return new Response('Authentication required', {
      status: 401,
      headers: { 'WWW-Authenticate': 'Basic realm="MEDEX Sprint Review"' }
    });
  }
}
export const config = { matcher: '/' };
```

---

## Opção 3: Deploy Manual (Drag & Drop)

1. Acesse [app.netlify.com/drop](https://app.netlify.com/drop)
2. Arraste a pasta `medex-sprint` (com o `index.html` dentro)
3. Pronto — URL gerada em 10 segundos
4. Vá em Settings → Password Protection para proteger

---

## Credenciais
- **Login:** medex
- **Password:** sprint2026

## Checklist Pré-Deploy
- [x] `index.html` é self-contained (< 500KB)
- [x] Fonts carregam via Google Fonts CDN
- [x] ApexCharts carrega via CDN
- [x] Sem dependências locais
- [x] Responsivo (1920x1080 + mobile)
