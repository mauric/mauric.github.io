---
layout: post
title: "Patrones de diseño"
date:   2025-10-21
categories: design patterns
permalink: /:categories/:year/:month/:day
---


# ¿Qué son los patrones de diseño? 

Hay problemas que se presentan con frecuencia en el desarrollo de software. Estos problemas
tienen a veces formas parecidas lo que lleva a que puedan ser resueltos con soluciones similares.
Dada la frecuencia y la similitud de dichas soluciones es que podemos tener un conjunto de 
patrones de diseño que son estas soluciones que podemos aplicar a escenarios comunes dentro 
de un software que estemos desarrollando. Así como cuando compramos piezas mecánicas estandar
para resolver alguna fijación solo debemos tener en cuenta las medidas y especificaciones de dicha pieza pero sabiendo siempre que la función de dicha pieza será una buena solución para
la situación que estamos buscando resolver. 

Estos patrones no pueden ser implementados en código y luego ser copiados y pegados en 
cualquier otro programa o código en particular porque lo que se ajusta a la realidad y el 
contexto del problema en particular es el concepto del patron no el código o la implementación en sí. 

Para poner un ejemplo quiero pensar en que si hice cuatro agujeros en una pared para colgar un televisor y llamo a esto patron de fijación de televisor, esa implementación no me servirá si cambio a un televisor distinto o más grande y busco usar los mismo agujeros de la pared para fijarlo. Esto se debe a que probablemente exista un diferencia entre las distancias de los agujeros o la forma en la que están dispuestos. 
Por ende tengo que realizar una nueva implementación, perforando cuatro agujeros nuevos que se ajusten al contexto y realidad del nuevo televisor. 

Podemos encontrar algunas confusiones al escuchar los términos algoritmo y patron de diseño como sinónimos. Nada está más lejos de la realidad. Existe una gran diferencia entre estos conceptos. Un patrón es una descripción de la forma de una solución a problemas típicos y frecuentes. 
Un algoritmo es el conjunto de instrucciones que deben ser ejecutadas para llegar aun propósito y resultado claros. 

Pensemos en un algoritmo como una receta de cocina y un patron como un plano de ingeniería. 



## ¿En que consiste un patron? 

Los patrones se identifican de forma estructurada dado que buscamos que muchas personas comprendan lo mismo al momento de hablar sobre como deberían funcionar. Cualquier descrición un poco vaga o ambigua causaría que dos personas entablen una discusión sobre que nombre debería tener lo que tiene conceptualmente la misma forma y propósito. 

Para diferencialos usaremos algunos puntos característicos como por ejemplo:
- Propósito: problema y solución 
- Motivación: detalle de la solución
- Estructura: conjunto de clases y como se relacionan
- Pseudo código descriptivo: Y si vamos a querer que alguien escriba un ejemplo al menos en 
pseudo código (creo que en este artículo voy a usar C++ para los ejemplos)

## Un toque de historia 

Las disciplinas siempre toman conceptos de otras cuando estos se vuelven populares o revolucionarios. En el caso de los patrones de diseño la idea se tomó de un libro llamado
"El lenguaje de patrones" el cual trataba de establecer relaciones entre las distancias 
y proporciones y como estos definían los espacios para darle así crear un "lenguaje" que 
permitiera dar forma a espacios urbanos y arquitectónicos. 

Esta idea está genial y fue tomada por cuatro autores americanos en 1995. De la misma forma 
que estos conceptos se popularizaron y resistiendo al tiempo fueron pulidos por la comunidad
hasta hoy donde podemos decir que son parte tanto del lenguaje de programados que están 
tratando de buscarle una solución a un problema como un standard en el diseño de software. 

## ¿Por qué debería aprender estos patrones?

Si vas a encarar una solución deberías estar seguro de que no estás reinventando la rueda. Si tiene forma de patrón usa el patrón, talvez lo tengas que ajustar un poco (limarle un 
poco el costado para que entre y que no se rompa luego - sería una buena analogía mecánica). 
Otra razón es que tantos tus colegas, miembros de tu equipo o comunidad van a buscar 
comunicarse con herramientas estandar sobre todo si no hablan el mismo idioma. Entonces no 
solo son conceptos sino que es lenguaje. 
- "Esto que han implementado en esta librería tien forma de Adapter con un bridge" podría ser un ejemplo de una conversación mientras buscamos entender el código 
que otra persona a escrito. 

## Contraargumento para NO utilizar patrones

Al ser abstracciones que resuelven complejidades frecuentes los patrones pueden llevar 
a programas con tamaños indeseados o incluso ineficientes. 
Otro escenario que no es ideal es cuando se utilizan patrones de manera excesiva o para tamaños de software que no se justifican. 
**Volcando esta indesición por sacrificar un poco de estandarización por un código más eficiente y personalizado a un uso excesivo de recursos o de complejidad en el código 
que luego puede traducirse silenciosamente en tiempo mal utilizado por los desarrolladores**


# Clasificación de patrones de diseño

Según su nivel de abstracción alto o bajo podemos decir que los que están en el fondo de la 
escala y que estan ligados fuertemente al lenguajes son los llamados **idioms**. En 
contraparte veremos los patrones de más alto nivel que pueden ser implementados en cualquier
lenguaje de programación a los **patrones de arquitectura**

Fuera del nivel de abstracción encontramos una clasificación por *propósito* (que hace y como?):
1 - Patrones creacionales: creación de objetos y reutilización de código
2 - Patrones estructurales: ensamblar objetos en estructuras más amplias manteniendo eficiencia
3 - Patrones de comportamiento: comunicación efectiva y asignar responsabilidades. Patrones que funcionan como las grandes corporaciones. 