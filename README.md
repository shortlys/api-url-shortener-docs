# 📜 Documentação Técnica da API - Slnk.bio

_📌 Versão 1.0.0 | Última atualização: 15/01/2025

---

## 📌 Introdução

A API do **Slnk.bio** permite encurtar URLs de forma rápida e segura, utilizando requisições HTTP nos métodos **POST** e **GET**.

Este documento fornece detalhes sobre a implementação, exemplos de código e integração com **React** e **Axios**.

---

## 🔹 Método POST

### 🔧 Implementação do Formulário

O exemplo abaixo demonstra como criar um formulário HTML para enviar uma URL e obter o link encurtado via **POST**.

```html
<!-- Formulário para encurtar URLs usando o método POST -->
<form id="urlForm" method="POST" action="https://slnk.bio/api.php">
  <!-- Campo para inserir a URL completa -->
  <input type="text" name="url" id="url" placeholder="URL completa" required />

  <!-- Campo para inserir a chave da API -->
  <input type="text" name="api_key" id="api_key" required />

  <!-- Botão para enviar o formulário -->
  <button type="submit">Encurtar</button>
</form>

<!-- Div onde o resultado será exibido -->
<div id="result"></div>
```

### 🔑 Adicionando a Chave API Padrão

Para tornar o uso da API mais dinâmico, você pode definir uma chave padrão no formulário, conforme o exemplo abaixo:

```html
<input
  type="hidden"
  name="api_key"
  id="api_key"
  required
  value="jbhaisnkaksm5544a6s6as64aa66"
/>
```

> **Nota:** O campo de entrada está definido como `hidden`, tornando a experiência mais intuitiva para o usuário.

### 🛠️ Configuração JavaScript

Para evitar conflitos, o script deve ser carregado no `<head>` com o atributo `defer` ou inserido no final do `<body>`.

```html
<script src=".
https://slnk.bio/api/assets/scripts/v1/post/snlk.bio.js"></script>
```

---

## 🔹 Método GET

### 🔍 Estrutura da Requisição

```
GET https://slnk.bio/api.php?url={URL}&api_key={CHAVE_API}
```

### 🖥️ Exemplo com cURL

```bash
curl "https://slnk.bio/api.php?url=https://exemplo.com&api_key=sua_chave_aqui"
```

### 🔧 Implementação do Formulário

```html
<!-- Formulário para encurtar URLs usando o método GET -->
<form id="urlForm" method="GET" action="https://slnk.bio/api.php">
  <!-- Campo para inserir a URL completa -->
  <input type="text" name="url" id="url" placeholder="URL completa" required />

  <!-- Campo para inserir a chave da API -->
  <input type="text" name="api_key" id="api_key" required />

  <!-- Botão para enviar o formulário -->
  <button type="submit">Encurtar</button>
</form>

<!-- Div onde o resultado será exibido -->
<div id="result"></div>
```

### 🔑 Adicionando a Chave API Padrão

```html
<input
  type="hidden"
  name="api_key"
  id="api_key"
  required
  value="jbhaisnkaksm5544a6s6as64aa66"
/>
```

> **Nota:** O campo `hidden` mantém a interface mais limpa para o usuário.

### 🛠️ Configuração JavaScript

```html
<script src="https://slnk.bio/api/assets/scripts/v1/get/snlk.bio.js"></script>
```

### 📩 Modelo de Resposta

#### ✅ Sucesso

```json
{
  "success": "URL encurtada com sucesso",
  "shortenedUrl": "slnk.bio/is/abc123"
}
```

#### ❌ Erro

```json
{
  "error": "Chave de API inválida"
}
```

---

## 🔹 Integração com React

### ⚛️ Componente Básico

```jsx
import React, { useState } from "react";

const UrlShortener = () => {
  const [url, setUrl] = useState("");
  const [apiKey, setApiKey] = useState("");
  const [result, setResult] = useState(null);
  const [loading, setLoading] = useState(false);

  const handleSubmit = async (e) => {
    e.preventDefault();
    setLoading(true);

    try {
      const formData = new FormData();
      formData.append("url", url);
      formData.append("api_key", apiKey);

      const response = await fetch("https://slnk.bio/api.php", {
        method: "POST",
        body: formData,
      });

      const data = await response.json();
      setResult(data);
    } catch (error) {
      setResult({ error: "Erro na conexão" });
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className="max-w-md mx-auto">
      <form onSubmit={handleSubmit} className="space-y-4">
        <input
          type="text"
          value={url}
          onChange={(e) => setUrl(e.target.value)}
          placeholder="Insira a URL"
          className="w-full p-2 border rounded"
          required
        />
        <input
          type="text"
          value={apiKey}
          onChange={(e) => setApiKey(e.target.value)}
          placeholder="Chave API"
          className="w-full p-2 border rounded"
          required
        />
        <button
          type="submit"
          className="w-full bg-blue-500 text-white p-2 rounded hover:bg-blue-600"
          disabled={loading}
        >
          {loading ? "Encurtando..." : "Encurtar URL"}
        </button>
      </form>

      {result && (
        <div
          className={`mt-4 p-4 rounded ${
            result.error ? "bg-red-100" : "bg-green-100"
          }`}
        >
          {result.error ? (
            <p className="text-red-700">Erro: {result.error}</p>
          ) : (
            <>
              <p className="text-green-700">{result.success}</p>
              <p className="mt-2">
                URL Encurtada:{" "}
                <a href={result.shortenedUrl} className="text-blue-600">
                  {result.shortenedUrl}
                </a>
              </p>
            </>
          )}
        </div>
      )}
    </div>
  );
};

export default UrlShortener;
```

---

## 🔹 Uso com Axios

```javascript
import axios from "axios";

// Método POST
const shortenUrl = async (url, apiKey) => {
  try {
    const response = await axios.post(
      "https://slnk.bio/api.php",
      { url, api_key: apiKey },
      { headers: { "Content-Type": "multipart/form-data" } }
    );
    return response.data;
  } catch (error) {
    throw error.response.data;
  }
};

// Método GET
const shortenUrlGet = async (url, apiKey) => {
  try {
    const response = await axios.get("https://slnk.bio/api.php", {
      params: { url, api_key: apiKey },
    });
    return response.data;
  } catch (error) {
    throw error.response.data;
  }
};
```

---

## 🔹 Referência de Códigos

| Código | Descrição                      | Solução                                |
| ------ | ------------------------------ | -------------------------------------- |
| `400`  | Parâmetros ausentes            | Verificar parâmetros obrigatórios      |
| `401`  | Chave API inválida             | Validar chave no painel administrativo |
| `429`  | Limite de requisições excedido | Aguardar renovação do ciclo            |

---

## 🔹 Considerações Técnicas

- Todas as requisições devem utilizar **HTTPS**
- **Encoding:** UTF-8 obrigatório
- **Formato de resposta:** JSON
- **Timeout:** 15 segundos por requisição
- **Logs de acesso:** Disponíveis no painel administrativo
