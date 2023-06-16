# **K6: Herramienta de Pruebas de Rendimiento**

## ¿Qué es k6?

k6 es una herramienta de código abierto diseñada para pruebas de rendimiento y carga en aplicaciones web. Permite a los desarrolladores simular tráfico hacia una aplicación o sitio web y medir su rendimiento bajo diferentes tipos de carga. A diferencia de muchas otras herramientas de pruebas de carga, k6 está diseñado para ser utilizado por desarrolladores en su flujo de trabajo de desarrollo de software, y puede ser incorporado en sus pipelines de CI/CD.

## Instalación de k6

La instalación de k6 es bastante sencilla y se puede hacer en varias plataformas. A continuación se describe cómo instalar k6 en Windows, Linux y Mac.

### Windows

Para Windows, la forma más fácil de instalar k6 es a través de [Chocolatey](https://chocolatey.org/):

```bash
choco install k6
```

### Linux

En sistemas Linux, puedes instalar k6 utilizando [apt](https://www.apt.org/):

```bash
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 379CE192D401AB61
echo "deb https://dl.bintray.com/loadimpact/deb stable main" | sudo tee -a /etc/apt/sources.list
sudo apt-get update
sudo apt-get install k6
```

### Mac

En macOS, puedes instalar k6 con [Homebrew](https://brew.sh/):

```bash
brew install k6
```

## Ventajas y Desventajas de usar k6

### Ventajas

1. **Facilidad de uso**: k6 es fácil de usar e instalar. Su API es clara y bien documentada.
2. **Basado en JavaScript**: k6 utiliza JavaScript, un lenguaje muy conocido y accesible para los desarrolladores.
3. **Integración CI/CD**: k6 está diseñado para integrarse fácilmente con las tuberías de CI/CD, lo que permite pruebas de carga continuas.
4. **Código abierto**: k6 es de código abierto, lo que significa que puedes contribuir al proyecto y adaptarlo a tus necesidades.

### Desventajas

1. **Rendimiento**: Dado que k6 está basado en JavaScript, puede no ser tan eficiente como las herramientas basadas en lenguajes de bajo nivel como C o Rust.
2. **Necesidad de conocimientos de JavaScript**: Aunque JavaScript es un lenguaje popular, puede ser una barrera de entrada para aquellos que no están familiarizados con él.
3. **Menos características que otras herramientas**: Comparado con algunas otras herramientas de pruebas de carga, k6 puede tener menos características.

## Pruebas de Rendimiento con k6

### ¿Qué es una Prueba de Carga?

Una prueba de carga es una forma de prueba de rendimiento donde se simula el uso de muchos usuarios para entender cómo se comportará la aplicación bajo cargas normales o altas. El objetivo es identificar cuellos de botella y garantizar que la aplicación pueda manejar el volumen de usuarios previsto.

### ¿Cómo implementar una Prueba de Carga en k6?

Para implementar una prueba de carga con k6, primero debes definir el comportamiento de los usuarios virtuales (VU) en un script de JavaScript. A continuación, se muestra un ejemplo sencillo:

```javascript
import http from 'k6/http';


import { sleep } from 'k6';

export let options = {
    vus: 10,
    duration: '30s',
};

export default function () {
    http.get('http://test.k6.io');
    sleep(1);
}
```

En este script, estamos definiendo 10 VU que harán solicitudes GET a `http://test.k6.io` durante 30 segundos. Cada VU hará una solicitud y luego dormirá durante 1 segundo antes de hacer la siguiente solicitud.

### ¿Qué es una Prueba de Estrés?

Una prueba de estrés es un tipo de prueba de rendimiento que pone la aplicación bajo condiciones extremas, como grandes volúmenes de tráfico o datos. El objetivo es entender los límites de la aplicación y ver cómo se comporta cuando se superan esos límites.

### ¿Cómo implementar una Prueba de Estrés en k6?

Para realizar una prueba de estrés en k6, puedes incrementar el número de usuarios virtuales (VU) y/o el volumen de datos que tu aplicación tiene que manejar. Aquí tienes un ejemplo:

```javascript
import http from 'k6/http';
import { sleep } from 'k6';

export let options = {
    stages: [
        { duration: '2m', target: 100 }, // subir a 100 VU durante 2 minutos
        { duration: '5m', target: 100 }, // mantener 100 VU durante 5 minutos
        { duration: '2m', target: 0 }, // reducir a 0 VU durante 2 minutos
    ],
};

export default function () {
    http.get('http://test.k6.io');
    sleep(1);
}
```

Este script realiza una prueba de estrés que comienza con 0 VU, aumenta a 100 VU durante 2 minutos, mantiene esos 100 VU durante 5 minutos y luego reduce a 0 VU durante 2 minutos.

### ¿Qué es una Prueba de Punto de Ajuste?

Una prueba de punto de ajuste es un tipo de prueba de rendimiento que ayuda a identificar el punto en el que añadir más carga a la aplicación no resulta en un aumento de rendimiento. Este tipo de prueba ayuda a identificar el punto óptimo de escala para tu aplicación.

### ¿Cómo implementar una Prueba de Punto de Ajuste en k6?

Implementar una prueba de punto de ajuste en k6 implica aumentar gradualmente la carga en la aplicación hasta que se observe que el rendimiento no mejora o incluso disminuye. Aquí te mostramos un ejemplo de cómo se podría hacer:

```javascript
import http from 'k6/http';
import { sleep } from 'k6';

export let options = {
    stages: [
        { duration: '2m', target: 50 }, // subir a 50 VU
        { duration: '5m', target: 100 }, // subir a 100 VU
        { duration: '2m', target: 200 }, // subir a 200 VU
        // Sigue incrementando los VU hasta que el rendimiento ya no mejore
    ],
};

export default function () {
    http.get('http://test.k6.io');
    sleep(1);
}
```

### ¿Qué es una Prueba de Pico?

Una prueba de pico es un tipo de prueba de rendimiento que simula una carga de tráfico abrupta y extremadamente alta en un corto período de tiempo. El objetivo es verificar si la aplicación puede

 manejar picos súbitos en el tráfico.

### ¿Cómo implementar una Prueba de Pico en k6?

Para implementar una prueba de pico en k6, puedes usar un patrón de carga que aumenta el número de VU muy rápidamente. Aquí te mostramos un ejemplo:

```javascript
import http from 'k6/http';
import { sleep } from 'k6';

export let options = {
    stages: [
        { duration: '1m', target: 2000 }, // subir a 2000 VU en 1 minuto
        { duration: '3m', target: 2000 }, // mantener 2000 VU durante 3 minutos
        { duration: '1m', target: 0 }, // reducir a 0 VU en 1 minuto
    ],
};

export default function () {
    http.get('http://test.k6.io');
    sleep(1);
}
```

Este script realiza una prueba de pico que comienza con 0 VU, aumenta rápidamente a 2000 VU en 1 minuto, mantiene esos 2000 VU durante 3 minutos, y luego los reduce a 0 en 1 minuto.
