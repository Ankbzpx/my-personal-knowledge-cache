
## Incident light
Light falls on object surface

## Specular reflection
Mirror like reflection, determined by the angle between reflection vector(reflection of light vector on normal) and view vector (direction from view position to fragement position)

## Diffuse reflection
The reflection of light from a surface such that a ray incident on the surface is scattered as many angles (contrast to specular reflection). The brightness is the same regardless of viewing angle, but will change if surface is tilted away from light direction, as its incident light change. Usually calculated by dot product of light direction and normal.

## Albedo
1. Physics: The proportion of incident light that is reflected away from a surface
2. PBR: base color without shadows (Diffuse in non-PBR)

## Forward shading
![](/assets/images/2021-12-21-10-34-51.png)

## Deferred shading (G-Buffer):
![](/assets/images/2021-12-21-10-35-02.png)

## Phong lighting model
> Reference: https://learnopengl.com/Lighting/Basic-Lighting

`(ambient + diffuse + specular) * lightColor`

- Ambient: constant
```
    float ambientStrength = 0.1;
    vec3 ambient = ambientStrength * lightColor;
```

- Diffuse: simulate [[diffuse reflection|rendering.rendering-terms#diffuse-reflection]]

```
    vec3 norm = normalize(Normal);
    vec3 lightDir = normalize(lightPos - FragPos);
    float diff = max(dot(norm, lightDir), 0.0);
    vec3 diffuse = diff * lightColor;
```

- Specular: simulate [[specular reflection|rendering.rendering-terms#specular-reflection]]

```
    float specularStrength = 0.5;
    vec3 viewDir = normalize(viewPos - FragPos);
    vec3 reflectDir = reflect(-lightDir, norm);
    float spec = pow(max(dot(viewDir, reflectDir), 0.0), 32);
    vec3 specular = specularStrength * spec * lightColor;
```