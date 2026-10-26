# player
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;
 
public class Player : MonoBehaviour
{
    //Salud actual del jugador
    private int health = 10;
 
    //Cantidad de monedas recolectadas
    private int coins;
 
    //El prefab Fireball y el parámetro Transform del punto de ataque
    public GameObject fireballPrefab;
    public Transform attackPoint;
 
 
   //El componente responsable de reproducir sonido
    public AudioSource audioSource;
 
    //Archivo de sonido que contiene el efecto de sonido de daño
    public AudioClip damageSound;
 
    //El método que procesa el daño realizado
    public void TakeDamage(int damage)
    {
        health -= damage;
 
        //Si aún queda salud, se reproduce el sonido de daño
        if(health > 0)
        {
            audioSource.PlayOneShot(damageSound);
            //print("Player health: " + health);
        }
        //Si no queda salud, entonces se reinicia la escena actual
        else
        {
            int sceneIndex = SceneManager.GetActiveScene().buildIndex;
            SceneManager.LoadScene(sceneIndex);
        }
    }
 
    //Método que aumenta la cantidad de monedas
    public void CollectCoins()
    {
        coins++;
 print("Collected coins: " + coins);
    }
 
 
    void Update()
    {
 
        //Si el jugador hace clic izquierdo con el mouse, se crea una bola de fuego
        if (Input.GetMouseButtonDown(0))
        {
            Instantiate(fireballPrefab, attackPoint.position, attackPoint.rotation);
        }
 
    }
}
 

Platform.cs

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
 
public class Platform : MonoBehaviour
{
    //Velocidad de movimiento de la plataforma
    public float speed;
    //Dirección de movimiento de la plataforma
    public Vector3 direction;
    //Estado de la plataforma: Activa / inactiva
    public bool isActive;
 
 
    //Actualizando el movimiento de la plataforma cada frame
    void Update()
    {
 
        if (isActive)
        {
 
            transform.position += direction * speed * Time.deltaTime;
        }
    }
 
 
    //Colisión de plataforma con dos tipos de objetos
    void OnTriggerEnter(Collider other) {
        /*Si la plataforma llega a un punto de parada, cambia la dirección
        de su movimiento*/
        if (other.tag == "PlatformStop")
        {
            direction *= -1;
        }
        //Si la plataforma es tocada por el jugador, se activa
        if(other.tag == "Player")
        {
            isActive = true;
        }
    }
 
    void OnTriggerExit(Collider other) {
       //Si un jugador ha dejado la plataforma, está desactivada
        if (other.tag == "Player")
        {
            isActive = false;
        }
    } 
 
}



Timer.cs

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;
 
public class Timer : MonoBehaviour
{
    public int minutes;
    public float seconds;
 
    //El ciclo de actualización es de aproximadamente 0.01 segundos
    void Update()
    {
        seconds -= Time.deltaTime;
 
        if (seconds <= 0)
        {
            if (minutes > 0)
            {
                seconds += 59;
 
                minutes--;
            }
            else
            {
                //Si el temporizador se ha detenido, recargar la escena actual
                int sceneIndex = SceneManager.GetActiveScene().buildIndex;
                SceneManager.LoadScene(sceneIndex);
            }
        }
    }
}
