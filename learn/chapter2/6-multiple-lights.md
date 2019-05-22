# Multiple lights
In the previous tutorials we learned quite a lot about lighting in OpenGL. We learned about Phong shading, materials, lighting maps and different types of light casters. In this tutorial we're going to combine all the previously obtained knowledge by creating a fully lit scene with 6 active light sources. We are going to simulate a sun-like light as a directional light source, 4 point lights scattered throughout the scene and we'll be adding a flashlight as well.

To use more than one light source in the scene we want to encapsulate the lighting calculations into GLSL **functions**. The reason for that is that the code quickly gets nasty when we want to do lighting computations with multiple lights with each light type requiring different computations. If we were to do all these calculations in the **main** function only, the code quickly becomes difficult to understand.

Functions in GLSL are just like C-functions. We have a function name and a return type. Except in GLSL we have to declare a prototype at the top of the code file if the function hasn't been declared before the main function yet. We'll create a different function for each of the light types: directional lights, point lights and spotlights.

When using multiple lights in a scene the approach is usually as follows: we have a single color vector that represents the fragment's output color. For each light, the light's contribution color of the fragment is added to the fragment's output color vector. So each light in the scene will calculate its individual impact on the aforementioned fragment and contribute to the final output color. A general structure would look something like this:

```glsl
out vec4 FragColor;

void main()
{
  // define an output color value
  vec3 output = vec3(0.0);
  // add the directional light's contribution to the output
  output += someFunctionToCalculateDirectionalLight();
  // do the same for all point lights
  for(int i = 0; i < nr_of_point_lights; i++)
  	output += someFunctionToCalculatePointLight();
  // and add others lights as well (like spotlights)
  output += someFunctionToCalculateSpotLight();
  
  FragColor = vec4(output, 1.0);
}
```
The actual code will likely differ per implementation, but the general structure remains the same. We define several functions that calculate the impact per light source and add its resulting color to an output color vector. If for example two light sources are close to the fragment, their combined contribution would result in a more brightly lit fragment than the fragment being lit by a single light source.

## Directional light
What we want to do is define a function in the fragment shader that calculates the contribution a directional light has on the corresponding fragment: a function that takes a few parameters and returns the calculated directional lighting color.

First we need to set the required variables that we minimally need for a directional light source. We can store the variables in a struct called DirLight and define it as a uniform. The required variables should be familiar from the previous tutorial:

```glsl
struct DirLight {
    vec3 direction;
  
    vec3 ambient;
    vec3 diffuse;
    vec3 specular;
};  
uniform DirLight dirLight;
```
We can then pass the dirLight uniform to a function with the following prototype:

```glsl
vec3 CalcDirLight(DirLight light, vec3 normal, vec3 viewDir);
```
> Just like C and C++ if we want to call a function (in this case inside the **main** function) the function should be defined somewhere before the caller's line number. In this case we'd prefer to define the functions below the **main** function so this requirement doesn't hold. Therefore we declare the function's prototypes somewhere above the **main** function, just like we would in C.

You can see that the function requires a DirLight struct and two other vectors required for its computation. If you successfully completed the previous tutorial then the content of this function should come as no surprise:

```glsl
vec3 CalcDirLight(DirLight light, vec3 normal, vec3 viewDir)
{
    vec3 lightDir = normalize(-light.direction);
    // diffuse shading
    float diff = max(dot(normal, lightDir), 0.0);
    // specular shading
    vec3 reflectDir = reflect(-lightDir, normal);
    float spec = pow(max(dot(viewDir, reflectDir), 0.0), material.shininess);
    // combine results
    vec3 ambient  = light.ambient  * vec3(texture(material.diffuse, TexCoords));
    vec3 diffuse  = light.diffuse  * diff * vec3(texture(material.diffuse, TexCoords));
    vec3 specular = light.specular * spec * vec3(texture(material.specular, TexCoords));
    return (ambient + diffuse + specular);
}
```
We basically copied the code from the previous tutorial and used the vectors given as function arguments to calculate the directional light's contribution vector. The resulting ambient, diffuse and specular contributions are then returned as a single color vector.

## Point light
Just like with directional lights we also want to define a function that calculates the contribution a point light has on the given fragment, including its attenuation. Just like directional lights we want to define a struct that specifies all the variables required for a point light:

```glsl
struct PointLight {    
    vec3 position;
    
    float constant;
    float linear;
    float quadratic;  

    vec3 ambient;
    vec3 diffuse;
    vec3 specular;
};
#define NR_POINT_LIGHTS 4
uniform PointLight pointLights[NR_POINT_LIGHTS];
```
As you can see we used a pre-processor directive in GLSL to define the number of point lights we want to have in our scene. We then use this **NR_POINT_LIGHTS** constant to create an array of **PointLight** structs. Arrays in GLSL are just like C# arrays and can be created by the use of two square brackets. Right now we have 4 **PointLight** structs to fill with data.

> We could also simply define **one** large struct (instead of different structs per light type) that contains **all** the necessary variables for all the different light types and use that struct for each function, and simply ignore the variables we don't need. However, I personally find the current approach more intuitive and aside from a few extra lines of code it could save up some memory since not all light types need all variables.

The prototype of the point light's function is as follows:

```glsl
vec3 CalcPointLight(PointLight light, vec3 normal, vec3 fragPos, vec3 viewDir);
``` 

The function takes all the data it needs as its arguments and returns a vec3 that represents the color contribution that this specific point light has on the fragment. Again, some intelligent copy-and-pasting results in the following function:

```glsl
vec3 CalcPointLight(PointLight light, vec3 normal, vec3 fragPos, vec3 viewDir)
{
    vec3 lightDir = normalize(light.position - fragPos);
    // diffuse shading
    float diff = max(dot(normal, lightDir), 0.0);
    // specular shading
    vec3 reflectDir = reflect(-lightDir, normal);
    float spec = pow(max(dot(viewDir, reflectDir), 0.0), material.shininess);
    // attenuation
    float distance    = length(light.position - fragPos);
    float attenuation = 1.0 / (light.constant + light.linear * distance + 
  			     light.quadratic * (distance * distance));    
    // combine results
    vec3 ambient  = light.ambient  * vec3(texture(material.diffuse, TexCoords));
    vec3 diffuse  = light.diffuse  * diff * vec3(texture(material.diffuse, TexCoords));
    vec3 specular = light.specular * spec * vec3(texture(material.specular, TexCoords));
    ambient  *= attenuation;
    diffuse  *= attenuation;
    specular *= attenuation;
    return (ambient + diffuse + specular);
}
```
Abstracting this functionality away in a function like this has the advantage that we can easily calculate the lighting for multiple point lights without the need for nasty duplicated code. In the **main** function we simply create a loop that iterates over the point light array that calls **CalcPointLight** for each point light.

## Putting it all together
Now that we defined both a function for directional lights and a function for point lights we can put it all together in the main function.

```glsl
void main()
{
    // properties
    vec3 norm = normalize(Normal);
    vec3 viewDir = normalize(viewPos - FragPos);

    // phase 1: Directional lighting
    vec3 result = CalcDirLight(dirLight, norm, viewDir);
    // phase 2: Point lights
    for(int i = 0; i < NR_POINT_LIGHTS; i++)
        result += CalcPointLight(pointLights[i], norm, FragPos, viewDir);    
    // phase 3: Spot light
    //result += CalcSpotLight(spotLight, norm, FragPos, viewDir);    
    
    FragColor = vec4(result, 1.0);
}
```
Each light type adds its contribution to the resulting output color until all light sources are processed. The resulting color contains the color impact of all the light sources in the scene combined. If you want you could also implement a spotlight and add its effect to the output color as well. We leave the **CalcSpotLight** function as an exercise for the reader, (solution in source).

Setting the uniforms for the directional light struct shouldn't be too unfamiliar, but you might be wondering how we could set the uniform values of the point lights since the point light uniform is now an array of **PointLight** structs. This isn't something we've discussed before.

Luckily for us, it isn't too complicated. To set the uniform of an array of structs works just like setting the uniforms of a single struct, although this time we also have to define the appropriate index when querying the uniform's location:

```glsl
_lightingShader.setFloat("pointLights[0].constant", 1.0f);
```
Here we index the first **PointLight** struct in the **pointLights** array and retrieve the location of its *constant* variable. This does mean unfortunately that we have to manually set all the uniforms for each of the 4 point lights, which leads up to 28 uniform calls for the point lights alone which is a bit tedious. You could try to abstract a bit away from this by defining a point light class that sets the uniforms for you, but in the end you'd still have to set the all the lights' uniform values this way.

Let's not forget that we also need to define a position vector for each of the point lights so let's spread them up a bit around the scene. We'll define another Vector3 array that contains the pointlights' positions:

```cs
private readonly Vector3[] _pointLightPositions =
{
    new Vector3(0.7f, 0.2f, 2.0f),
    new Vector3(2.3f, -3.3f, -4.0f),
    new Vector3(-4.0f, 2.0f, -12.0f),
    new Vector3(0.0f, 0.0f, -3.0f)
};
```
Then index the corresponding **PointLight** struct from the **pointLights** array and set its position attribute as one of the *positions* we just defined. Also be sure to now draw 4 light cubes instead of just 1. Simply create a different model matrix for each of the light objects just like we did with the containers.

If you'd also use a flashlight the result of all the combined lights looks something like this:

![Multiple lights combined](img/6-multiple_lights_combined.png)

As you can see there appears to be some form of a global light (like a sun) somewhere in the sky, we have 4 lights scattered throughout the scene and a flashlight is visible from the player's perspective. Looks pretty neat doesn't it?

You can find the full source code of the final application [here](https://github.com/opentk/LearnOpenTK/tree/master/Chapter%202/6%20-%20Multiple%20lights).

The image shows all the light sources set with the default light properties we've used in all the previous tutorials, but if you'd play around with these values you can get pretty interesting results. Artists and level editors generally tweak all these lighting variables in a large editor to make sure the lighting matches the environment. Using the simple lighted environment we just created you can create some interesting visuals by simply tweaking the lights their attributes:

![Multiple lights atmospheres](img/6-multiple_lights_combined.png)

We also changed the clear color to better reflect the lighting. You can see that by simply adjusting some of the lighting parameters you can create completely different atmospheres.

By now you should have a pretty good understanding of lighting in OpenGL. With the knowledge so far we can already create interesting and visually rich environments and atmospheres. Try playing around with all the different values to create your own atmospheres.