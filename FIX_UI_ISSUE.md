# Fix para UI 404 - Guia Rápido

## 🐛 Problema Identificado
A imagem oficial do Docling Serve não inclui as dependências da UI (Gradio) por padrão, causando erro 404 em `/ui`.

## ✅ Solução Implementada

### Opção 1: Deploy Rápido (RECOMENDADO)
Usa imagem CPU-only com UI instalada automaticamente.

**Status atual do captain-definition**: `Dockerfile.cpu`

### O que foi modificado:

1. **`Dockerfile.cpu`**: Nova imagem otimizada
   - Usa `docling-serve-cpu` (mais leve)
   - Instala Gradio automaticamente
   - Configuração específica para porta 80
   - Variáveis de ambiente para UI

2. **`captain-definition`**: Aponta para `Dockerfile.cpu`

3. **Variáveis de ambiente configuradas**:
   ```env
   DOCLING_SERVE_ENABLE_UI=1
   GRADIO_SERVER_NAME=0.0.0.0
   GRADIO_SERVER_PORT=80
   ```

## 🚀 Para Testar Agora

1. **Commit e push das mudanças**:
   ```bash
   git add .
   git commit -m "Fix UI 404 - add Gradio dependencies"
   git push origin main
   ```

2. **No CapRover**:
   - Force redeploy do app
   - Aguarde 3-7 minutos
   - Teste: `https://docling-serve.platform.sinesys.app/ui`

## 🔄 Alternativas se não funcionar

### Opção A: Imagem completa
Edite `captain-definition`:
```json
{
  "schemaVersion": 2,
  "dockerfilePath": "./Dockerfile.simple"
}
```

### Opção B: Configuração manual no CapRover
Se a UI ainda não aparecer, adicione estas variáveis no CapRover:
- `DOCLING_SERVE_ENABLE_UI=1`
- `GRADIO_SERVER_NAME=0.0.0.0`

## 🔍 Debug

Se ainda houver 404, check logs no CapRover procurando por:
- `gradio` mentions
- `UI` initialization errors
- Port binding issues

## 📝 Próximos Passos
Após o deploy funcionar, os endpoints disponíveis serão:
- **API**: `https://docling-serve.platform.sinesys.app/docs`
- **UI**: `https://docling-serve.platform.sinesys.app/ui`
- **Health**: `https://docling-serve.platform.sinesys.app/health`