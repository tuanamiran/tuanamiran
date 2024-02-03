
{
    private CharacterController _controller;
    private Vector3 _velocity;
    private bool _isGrounded;
    [SerializeField] private GameObject _ground;
    [SerializeField] private float _jumpForce;
    [SerializeField] private float _walkSpeed;
    [SerializeField] private float _distance;
    [SerializeField] private float _gravity;=9.81f;
    [SerializeField] private LayerMask _mask;
    private float _limit;
    private Camera _mainCan;
    void Start()
    {
        _controller = GetComponent<CharacterController>();
        _mainCan = Camera.main;
        Cursor.visible = false;
        Cursor.lockState=CursorLockMode,Locked;

    }

   
    void Update()
    {
        _isGrounded = Physics.CheckSphere(_ground.transform.position, _distance, (int)_mask);
        if(_isGrounded&& _velocity.y<0)
        {
            _velocity.y = -2f;
        }
        float x = Input.GetAxisRaw("Horizontal");
        float y = Input.GetAxisRaw("Vertical");
        float x_look = Input.GetAxisRaw("Mause X");
        float y_look = Input.GetAxisRaw("Mause Y");
       
        _limit += y_look;
        _limit = Mathf.Clamp(value_limit, _minlimit, _maxlimit);
        _mainCan.transform.localEulerAngles = new Vector3(x: _limit, y: 0, z: 0);
        transform.Rotate(eulers: transform.up * (x_look * _sensitivity * Time.deltaTime));

        Vector3 direction = transform.right * x + transform.forward * y;
        _controller.Move(motion: direction * (_walkSpeed * Time.deltaTime));
        if(Input.GetButtonDown("Jump")&& _isGrounded)
        {
            _velocity.y = Mathf.Sqrt(f: _jumpForce * -2f * _gravity);
        }
        _velocity.y += _gravity * Time.deltaTime;
        _controller.Move(motion: _velocity * Time.deltaTime);
    }
}

