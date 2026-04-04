# Locale pt_BR.UTF-8 não gerado no Debian

## Sintoma

Ao conectar via SSH em um host Debian, aparecem os avisos:

```
bash: warning: setlocale: LC_ALL: cannot change locale (pt_BR.UTF-8): No such file or directory
locale: Cannot set LC_CTYPE to default locale: No such file or directory
locale: Cannot set LC_MESSAGES to default locale: No such file or directory
locale: Cannot set LC_ALL to default locale: No such file or directory
```

O comando `locale -a` não lista `pt_BR.utf8`.

---

## Por que acontece

O Debian separa a **configuração** do locale da sua **geração**:

| Arquivo | Função |
|---------|--------|
| `/etc/default/locale` | Define qual locale o sistema deve usar (ex: `LANG=pt_BR.UTF-8`) |
| `/etc/locale.gen` | Lista quais locales serão **compilados** pelo `locale-gen` |

O problema ocorre quando `/etc/default/locale` aponta para `pt_BR.UTF-8`, mas a linha correspondente em `/etc/locale.gen` está **comentada** (`#`). O `locale-gen` só compila os locales descomentados, então os arquivos de locale nunca são criados em `/usr/lib/locale/`.

**Situações comuns que causam isso:**
- Instalação mínima do Debian sem selecionar locale pt_BR
- Clone ou template de VM com locale incorreto
- Configuração manual de `LANG` sem rodar `locale-gen` depois

---

## Diagnóstico

```bash
# Ver o locale configurado (pode mostrar erros)
locale

# Ver locales disponíveis no sistema
locale -a

# Ver o que está definido como padrão
cat /etc/default/locale

# Ver quais locales estão habilitados para geração
grep -v '^#' /etc/locale.gen | grep -v '^$'
```

Se `pt_BR` aparece em `/etc/default/locale` mas **não** aparece em `locale -a`, o locale não foi gerado.

---

## Solução

### 1. Habilitar o locale no locale.gen

```bash
sudo sed -i 's/^# pt_BR.UTF-8 UTF-8/pt_BR.UTF-8 UTF-8/' /etc/locale.gen
```

Ou editar manualmente e descomentar a linha:

```
# pt_BR.UTF-8 UTF-8   →   pt_BR.UTF-8 UTF-8
```

### 2. Gerar o locale

```bash
sudo locale-gen
```

Output esperado:

```
Generating locales (this might take a while)...
  en_US.UTF-8... done
  pt_BR.UTF-8... done
Generation complete.
```

### 3. Verificar

Abrir nova sessão SSH e confirmar que não há mais avisos:

```bash
locale
locale -a   # deve listar pt_BR.utf8
```

---

## Ambiente onde foi aplicado

| Host | OS | Kernel | Data |
|------|----|--------|------|
| pve2.staypuff.info (192.168.3.12) | Debian GNU/Linux 13 (Trixie) | 6.17.13-2-pve | 2026-04-04 |
