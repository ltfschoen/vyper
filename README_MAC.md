# README

---
Vyper Setup on macOS WITH or WITHOUT Docker
---

Includes Troubleshooting tips and information about Vyper

# Table of Contents
  * [Chapter 0 - Setup WITHOUT Docker (including Troubleshooting)](#chapter-0)
  * [Chapter 1 - Setup WITH Docker](#chapter-1)
  * [Chapter 2 - Docker Containers and Images (Show/Delete)](#chapter-2)
  * [Chapter 3 - About Vyper](#chapter-3)
  * [Chapter 4 - Unit Tests](#chapter-4)

## Chapter 0 - Setup WITHOUT Docker <a id="chapter-0"></a>

* Install [PyEnv](https://github.com/pyenv/pyenv)
* Clone the repo
    ```bash
    git clone https://github.com/ethereum/vyper.git
    cd vyper;
    make;
    make test
    ```

* Troubleshooting
    * If you get an error such as:
        ```
        File "/private/var/folders/8q/kll0jy354kj5gxrv9n0pwx300000gn/T/easy_install-61r1e_jz/eth-hash-0.1.2/.eggs/setuptools_markdown-0.2-py3.6.egg/setuptools_markdown.py", line 22, in long_description_markdown_filename
        File "/private/var/folders/8q/kll0jy354kj5gxrv9n0pwx300000gn/T/easy_install-61r1e_jz/eth-hash-0.1.2/.eggs/pypandoc-1.4-py3.6.egg/pypandoc/__init__.py", line 66, in convert
        RuntimeError: Format missing, but need one (identified source as text as no file with that name was found).
        make: *** [Makefile:4: init] Error 1
        ```

    * And then you try fixing it by installing an older version of eth-hash with `pip3 install eth-hash==0.1.1` but it creates a new error:
        ```
        File "/private/var/folders/8q/kll0jy354kj5gxrv9n0pwx300000gn/T/easy_install-t0gzq4pa/hexbytes-0.1.0/.eggs/setuptools_markdown-0.2-py3.6.egg/setuptools_markdown.py", line 22, in long_description_markdown_filename
        File "/private/var/folders/8q/kll0jy354kj5gxrv9n0pwx300000gn/T/easy_install-t0gzq4pa/hexbytes-0.1.0/.eggs/pypandoc-1.4-py3.6.egg/pypandoc/__init__.py", line 66, in convert
        RuntimeError: Format missing, but need one (identified source as text as no file with that name was found).
        make: *** [Makefile:4: init] Error 1
        ```
    
    * Then make sure you read and action the warning message about upgrading your old pip version
        ```
        You are using pip version 9.0.1, however version 10.0.0 is available.
        You should consider upgrading via the 'pip install --upgrade pip' command.
        ```

    * Upgrade pip with the following
        ```bash
        pip install --upgrade pip
        ```

    * Try installing pypandoc. It might show a warning about packages dependencies that are required by weren't found after upgrading pip
        ```bash
        $ pip3 install pypandoc==1.4
        ```

        ```
        Requirement already satisfied: wheel>=0.25.0 in /Users/Me/.pyenv/versions/3.6.2/lib/python3.6/site-packages (from pypandoc==1.4) (0.31.0)
        web3 4.1.0 requires eth-abi<2,>=1.0.0, which is not installed.
        web3 4.1.0 requires eth-account==0.1.0-alpha.2, which is not installed.
        web3 4.1.0 requires hexbytes<1.0.0,>=0.1.0, which is not installed.
        eth-tester 0.1.0b21 requires semantic_version<3.0.0,>=2.6.0, which is not installed.
        cytoolz 0.9.0.1 requires toolz>=0.8.0, which is not installed.
        cryptography 2.2.2 requires asn1crypto>=0.21.0, which is not installed.
        cryptography 2.2.2 requires cffi>=1.7, which is not installed.
        cryptography 2.2.2 requires idna>=2.1, which is not installed.
        cryptography 2.2.2 requires six>=1.4.1, which is not installed.
        aiohttp 2.3.10 requires async_timeout>=1.2.0, which is not installed.
        aiohttp 2.3.10 requires chardet, which is not installed.
        aiohttp 2.3.10 requires idna-ssl>=1.0.0, which is not installed.
        aiohttp 2.3.10 requires multidict>=4.0.0, which is not installed.
        aiohttp 2.3.10 requires yarl>=1.0.0, which is not installed.
        requests 2.18.4 requires certifi>=2017.4.17, which is not installed.
        requests 2.18.4 requires chardet<3.1.0,>=3.0.2, which is not installed.
        requests 2.18.4 requires idna<2.7,>=2.5, which is not installed.
        requests 2.18.4 requires urllib3<1.23,>=1.21.1, which is not installed.
        ```

    * Install the missing dependencies

        ```bash
        pip3 install eth-abi==1.0.0 eth-account==0.1.0-alpha.2 hexbytes==0.1.0 semantic_version==2.6.0 toolz==0.8.0 asn1crypto==0.21.0 cffi==1.7 idna==2.5 six==1.4.1 async_timeout==1.2.0 idna-ssl==1.0.0 multidict==4.0.0 yarl==1.0.0 certifi==2017.4.17 chardet==3.0.2 urllib3==1.21.1
        ```

    * Run `make` worked successfully

        ```bash
        make
        ```

    * Try `make test`

        ```bash
        make test
        ```

        * Encountered error

        ```
        Running coincurve-7.1.0/setup.py -q bdist_egg --dist-dir /var/folders/8q/kll0jy354kj5gxrv9n0pwx300000gn/T/easy_install-ejiorwyq/coincurve-7.1.0/egg-dist-tmp-ihoar1u5
        build/temp.macosx-10.12-x86_64-3.6/_libsecp256k1.c:429:10: fatal error: 
            'secp256k1_ecdh.h' file not found
        #include <secp256k1_ecdh.h>
                ^~~~~~~~~~~~~~~~~~
        1 error generated.
        error: Setup script exited with error: command 'clang' failed with exit status 1
        make: *** [Makefile:7: test] Error 1
        ```

        * Tried changing to Coincurve 7.0.0 instead of 7.1.0
        ```
        pip3 install coincurve==7.0.0
        ```

        * New error
        ```
        File "/Users/Me/.pyenv/versions/3.6.2/lib/python3.6/site-packages/pkg_resources/__init__.py", line 719, in find
            raise VersionConflict(dist, req)
        pkg_resources.VersionConflict: (six 1.4.1 (/Users/Me/.pyenv/versions/3.6.2/lib/python3.6/site-packages), Requirement.parse('six>=1.10.0'))
        make: *** [Makefile:7: test] Error 1
        ```

        * Fixed with
        ```
        pip3 install six==1.10.0
        ```

    * Tried running `make test` again and it finally worked with all tests passing

    * Check what packages I had installed:

        ```bash
        $ pip3 list --local
        Package          Version  
        ---------------- ---------
        aiohttp          2.3.10   
        asn1crypto       0.21.0   
        async-lru        0.1.0    
        async-timeout    1.2.0    
        attrdict         2.0.0    
        certifi          2017.4.17
        cffi             1.7.0    
        chardet          3.0.2    
        coincurve        7.0.0    
        cryptography     2.2.2    
        cytoolz          0.9.0.1  
        eth-abi          1.0.0    
        eth-account      0.1.0a2  
        eth-bloom        1.0.0    
        eth-hash         0.1.1    
        eth-keyfile      0.5.1    
        eth-keys         0.2.0b3  
        eth-rlp          0.1.0    
        eth-tester       0.1.0b21 
        eth-utils        1.0.2    
        hexbytes         0.1.0    
        idna             2.5      
        idna-ssl         1.0.0    
        lru-dict         1.1.6    
        multidict        4.0.0    
        pip              10.0.0   
        py-ecc           1.4.2    
        py-evm           0.2.0a14 
        pycparser        2.18     
        pycryptodome     3.6.1    
        pyethash         0.1.27   
        pypandoc         1.4      
        requests         2.18.4   
        rlp              0.6.0    
        scrypt           0.8.6    
        semantic-version 2.6.0    
        setuptools       28.8.0   
        six              1.10.0   
        toolz            0.8.0    
        trie             1.3.4    
        urllib3          1.21.1   
        vyper            0.0.4    
        web3             4.1.0    
        websockets       4.0.1    
        wheel            0.31.0   
        yarl             1.0.0 
        ```

## Chapter 1 - Setup WITH Docker <a id="chapter-1"></a>

* Install and Run Docker
* Fork my repo https://github.com/ltfschoen/vyper
* Terminal #1 - Clone the fork

    ```bash
    cd ~;
    git clone https://github.com/ltfschoen/vyper;
    cd vyper;
    ```    

* Terminal #1 - Build the custom DockerfileMac. This allows file changes in the Docker container to be synchronised with your host machine and vice versa.

    ```bash
    docker build -t vyper:1 . -f DockerfileMac
    ```

* Terminal #2 - Create another Bash terminal window/tab in the same folder

* Terminal #2 - Open the directory in a Text Editor or IDE

* Terminal #1 - Start a shell session in the Docker container that you just created.
    
    ```bash
    docker run -it -v $(pwd):/code vyper:1 /bin/bash
    ```

* Terminal #1 (within Docker Container shell session) - Compile a Vyper contract
    ```bash
    vyper examples/crowdfund.v.py
    ```

* Make changes to examples/crowdfund.v.py in the Text Editor. The changes will also be reflected in the Docker Container.

* Terminal #1 - Repeat the previous command to try and re-compile the Vyper contract

## Chapter 2 - Docker Containers and Images (Show/Delete) <a id="chapter-2"></a>

* List all Docker containers 
    ```bash
    docker ps -a
    ```

* Stop all running containers. 
    ```bash
    docker stop $(docker ps -aq)
    ```

* Remove a specific Docker container
    ```bash
    docker rm <CONTAINER_ID>
    ```
* Remove a docker container 
    ```bash
    docker rm $(docker ps -aq)
    ```

* Remove all Docker images
    ```bash
    docker rmi $(docker images -q)
    ```

## Chapter 3 - About Vyper <a id="chapter-3"></a>

* Vyper Online Compilter (to bytecode or LLL) https://vyper.online/

* Vyper Features (vs Solidity)
    * Asserts instead of Modifiers
        * Pros
            * No arbitrary pre-conditions, no post-conditions
            * No arbitrary state changes
            * Less execution jumps for easier auditability
    * No Class Inheritance
    * No Function or Operator Overloading 
        * Pros
            * Safer since mitigates funds being stolen 
    * No Recursive Calling or Infinite-length Loops
        * Pros 
            * Avoids gas limit attacks since gas limit upper bound may be set
    * `.v.py` File Extension so Python syntax highlighting may be used
    * All Vyper syntax is valid Python 3 syntax, but not all Python 3 functionality is available in Vyper
    * Reference
        * http://viper.readthedocs.io/en/latest/


* Vyper Syntax where Files `.v.py` are a Smart Contract 
    * Class-like with:
        * State Variable
            * Usage: Permanently stored in contract Storage
            * Example
                ```python
                storedData: int128
                ```
        * [Types, Visibility, Getters](http://viper.readthedocs.io/en/latest/types.html#types)
            * TODO
        * Functions
            * Usage: 
                * Executable units in contract
                * Internally or Externally
                * Visibility-and-getters differ toward other contracts
                * Decorated with `@public` or `@private`
            * Example
                ```python
                @public
                @payable
                def bid(): // Function
                // ...
                ```
        * Events
            * Usage
                * Searchable by Clients and Light Clients since Events are logged in specially indexed data structures
                * Declared before global declarations and Function definitions
            * Example
                ```python
                Payment: event({amount: int128, arg2: indexed(address)})

                total_paid: int128

                @public
                def pay():
                    self.total_paid += msg.value
                    log.Payment(msg.value, msg.sender)
                ```
        * Structs
            * TODO

## Chapter 4 - Unit Tests <a id="chapter-4"></a>

```bash
python setup.py test
pip3 install ethereum==2.3.1 pytest pytest-cov pytest-runner
pytest
```