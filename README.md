# Implementación de Jenkins y Github Actions en aplicacion docker
## Jenkins
Configuración básica para build de imagen, push a dockerhub y deploy en servidor local                          
Además cuenta con 2 pasos adicionales mejorar su eficiencia                                                                 

Build -> Hadolint -> Push -> Deploy -> Test                                           

Hadolint se encarga de garantizar las mejores prácticas dentro del dockerfile, y mediante el uso del plugin HTTP request Jenkins prueba el correcto funcionamiento de la aplicación luego del deploy 

## Actions                                                                             

Workflow preparado para build de imagen y push a dockerhub                            
Además cuenta con el paso extra de Hadolint antes del build

Hadolint -> Build -> Push

