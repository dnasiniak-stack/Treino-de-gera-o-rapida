# Gerando o AAB assinado para a Play Store

A Google Play exige um arquivo .aab (Android App Bundle) assinado com uma
chave própria — o app-debug.apk do outro workflow não serve para publicar.

## Passo 1: gerar a keystore (uma vez só, e guardar para sempre)

No seu computador, com Java instalado, rode:

```
keytool -genkeypair -v -keystore release.keystore -alias meu-treino -keyalg RSA -keysize 2048 -validity 10000
```

Vai pedir uma senha da keystore, uma senha da chave e alguns dados (nome,
organização, país). Guarde o arquivo `release.keystore` e as duas senhas em
local seguro — sem eles você não consegue publicar atualizações do app no
futuro.

## Passo 2: converter a keystore para base64

```
base64 -i release.keystore -o release.keystore.base64
```

Abra o arquivo `release.keystore.base64` e copie todo o conteúdo (é um
texto longo).

## Passo 3: cadastrar os secrets no GitHub

No repositório, vá em **Settings > Secrets and variables > Actions** e crie:

- `KEYSTORE_BASE64`: cole o conteúdo do passo 2
- `KEYSTORE_PASSWORD`: a senha da keystore
- `KEY_ALIAS`: `meu-treino` (ou o alias que você escolheu)
- `KEY_PASSWORD`: a senha da chave

## Passo 4: rodar o workflow

Na aba **Actions**, escolha **Gerar AAB assinado (Play Store)** e clique em
**Run workflow**. Ao final, baixe o artefato **meu-treino-aab** — dentro
está o `app-release.aab`, pronto para subir no Play Console.

## Passo 5: no Play Console

1. Crie o app, preencha a ficha da loja (veja `ficha-da-loja.md`).
2. Envie o `app-release.aab` em **Produção > Criar nova versão**.
3. Cole o link da política de privacidade (`politica-de-privacidade.html`).
4. Preencha o questionário de classificação de conteúdo.
5. Defina o preço em **Preços e distribuição**.
6. Envie para revisão. A Google costuma levar de algumas horas a alguns
   dias para aprovar a primeira versão.
