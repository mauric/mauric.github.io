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

