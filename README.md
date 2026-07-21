using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;

public class Player : MonoBehaviour
{
    [SerializeField] Player player;

    public float walkSpeed = 0.01f;
    public float runSpeed = 0.05f;

    public float stamina = 5f;
    public float maxStamina = 5f;
    public float staminaRecovery = 1f;

    bool isRecovering = false;
    public AudioSource breathSound;

    // Update is called once per frame
    void Update()
    {
        float speed = walkSpeed;

        // 走る条件
        if (Input.GetKey(KeyCode.LeftShift) && stamina > 0 && !isRecovering)
        {
            speed = runSpeed;
            stamina -= Time.deltaTime;
        }
        else
        {
            stamina += staminaRecovery * Time.deltaTime;
        }

        // スタミナ0になったら回復モード
        if (stamina <= 0)
        {
            isRecovering = true;
        }

        // スタミナ満タンで回復終了
        if (stamina >= maxStamina)
        {
            isRecovering = false;
        }
        // スタミナ制限
        stamina = Mathf.Clamp(stamina, 0, maxStamina);

        // 息切れ音
        if (isRecovering)
        {
            if (!breathSound.isPlaying)
            {
                breathSound.Play();
            }
        }
        else
        {
            breathSound.Stop();
        }

        if (Input.GetKey(KeyCode.W))
        {
            transform.Translate(0, 0, speed);
        }

        if (Input.GetKey(KeyCode.S))
        {
            transform.Translate(0, 0, -speed);
        }

        if (Input.GetKey(KeyCode.A))
        {
            transform.Translate(-speed, 0, 0);
        }

        if (Input.GetKey(KeyCode.D))
        {
            transform.Translate(speed, 0, 0);
        }

        if (Input.GetKey(KeyCode.RightArrow))
        {
            transform.Rotate(0f, 0.1f, 0f);
        }

        if (Input.GetKey(KeyCode.LeftArrow))
        {
            transform.Rotate(0f, -0.1f, 0f);
        }
    }
}
