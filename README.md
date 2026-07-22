# DevLife — PWA (protótipo instalável)

Protótipo funcional do DevLife baseado no documento de requisitos (v1.1): hábitos,
gamificação (XP/nível/moedas), Pomodoro, estudos, treinos, peso, diário, calendário,
estatísticas e conquistas. Roda 100% no navegador — **sem backend, sem login real**.
Os dados ficam salvos no `localStorage` do navegador/dispositivo onde o app for aberto.

## O que É e o que NÃO é

- **É**: um app instalável (ícone na tela inicial, tela cheia, funciona offline depois
  de aberto uma vez), com toda a lógica do MVP funcionando de verdade no seu aparelho.
- **NÃO é**: um `.apk` nativo. Não tem servidor, não sincroniza entre aparelhos, não
  tem os RF20–RF26 (IA assistente) nem push notification de verdade (isso exige backend).
- Ver o Anexo A do documento de requisitos pra saber exatamente o que ficou de fora do MVP.

## Como instalar no celular (Android/Chrome)

Um arquivo HTML sozinho não vira "app instalável" — o navegador exige que ele esteja
sendo servido por http(s) pra ativar o Service Worker e o botão de instalar. Forma mais
simples e gratuita, usando o GitHub que você já usa:

1. Crie um repositório novo no GitHub (ex: `devlife-pwa`) e suba os arquivos desta pasta
   (`index.html`, `manifest.json`, `sw.js`, `icons/`) na raiz do repositório.
2. Vá em **Settings → Pages**, em "Source" escolha a branch `main` e pasta `/ (root)`. Salve.
3. Espere ~1 minuto. O GitHub te dá uma URL tipo `https://seu-usuario.github.io/devlife-pwa/`.
4. Abra essa URL no Chrome do celular → menu (⋮) → **"Adicionar à tela inicial"** (ou o
   Chrome pode sugerir isso sozinho). Pronto: vira um ícone de app, abre em tela cheia.

Alternativas igual de válidas: Netlify Drop (arrastar a pasta no netlify.com/drop) ou
Vercel — qualquer hospedagem estática funciona, não precisa ser o GitHub Pages.

## Rodando localmente pra testar antes de subir

```bash
cd devlife-pwa
python3 -m http.server 8080
# abra http://localhost:8080 no navegador
```

Abrir o `index.html` direto (`file://`) funciona pra ver a interface, mas o Service
Worker e o "Adicionar à tela inicial" só funcionam via http(s).

## Importante sobre a prévia dentro do Claude.ai

Se você estiver vendo este app rodando dentro de uma prévia do Claude (não hospedado
ainda), o `localStorage` é bloqueado pelo sandbox do navegador do Claude — então os
dados não vão persistir entre recarregamentos ali. Assim que você hospedar (GitHub
Pages, Netlify etc.) e abrir num navegador de verdade, a gravação de dados funciona
normalmente. Eu testei isso rodando em Chromium real durante o desenvolvimento e
confirmei que funciona (onboarding, hábitos, XP, Pomodoro, registros, calendário,
estatísticas e persistência após recarregar a página).

## Estrutura

```
devlife-pwa/
├── index.html      # app inteiro (HTML+CSS+JS, sem dependências externas)
├── manifest.json   # metadados do PWA (nome, ícone, cor)
├── sw.js           # service worker (cache offline do app shell)
├── icon-192.png
├── icon-512.png
└── icon-512-maskable.png
```

## Próximos passos sugeridos (fora do escopo deste protótipo)

- Backend real (Spring Boot + PostgreSQL, como no documento) pra sincronizar dados
  entre aparelhos e permitir login de verdade.
- IA Assistente (RF20–RF26) — precisa de um servidor pra chamar um modelo de linguagem.
- Push notification de verdade (Firebase Cloud Messaging ou Web Push) — hoje o app só
  notifica enquanto está aberto.
- Se decidir ir para Flutter (como especificado no documento original), esse protótipo
  serve como referência viva de fluxo e regras de negócio (fórmula de XP, cálculo de
  streak, conquistas) pra portar direto pro Dart.
