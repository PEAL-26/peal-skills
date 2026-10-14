# Frontend HTML-First & Vanilla JavaScript

Diretrizes para projetos e catálogos de animação executáveis sem etapa de build (HTML, CSS e Vanilla JS puros).

---

## 1. Princípios de Execução

- **Zero Build Step:** Uso direto de HTML5 semântico, CSS moderno e JavaScript vanilla modular. Sem empacotadores (webpack, vite) quando o objetivo for leveza e execução direta no navegador.
- **Hierarquia de Recursos Gráficos:**
  1. Prefira **CSS puro** e animações via keyframes.
  2. Em seguida, utilize **Web Animations API** ou **Canvas 2D**.
  3. Utilize **WebGL / Three.js** apenas quando efeitos tridimensionais ou shaders avançados forem indispensáveis.

---

## 2. Ciclo de Vida e Limpeza de Recursos (`mount` e `cleanup`)

Cada demonstração, efeito visual ou componente vanilla interativo deve expor obrigatoriamente um ciclo de vida controlado:

```javascript
export function mount(containerElement, options = {}) {
  // 1. Inicializar timers, listeners e animações
  const animationFrameId = requestAnimationFrame(renderLoop);
  window.addEventListener('resize', handleResize);

  // 2. Retornar ou registrar a função de cleanup
  return function cleanup() {
    cancelAnimationFrame(animationFrameId);
    window.removeEventListener('resize', handleResize);
    // Liberar geometrias, texturas e WebGL contexts se aplicável
  };
}
```

---

## 3. Acessibilidade e Performance

- **Movimento Reduzido:** Respeite rigorosamente a media query `prefers-reduced-motion: reduce`. Ofereça uma apresentação estática equivalente ou desative loops contínuos quando o usuário tiver essa preferência ativada.
- **Pausa Fora do Viewport:** Utilize `IntersectionObserver` ou verificação de visibilidade da página (`document.hidden`) para pausar imediatamente animações pesadas de Canvas e WebGL quando o elemento estiver fora da tela.
- **Persistência de Controles:** Armazene ajustes de sliders e configurações em um único objeto serializado no `localStorage` para hidratar controles antes do início de cada efeito.
