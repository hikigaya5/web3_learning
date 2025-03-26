La **estructura de un proyecto en Foundry** está diseñada para ser **minimalista, modular y eficiente**, optimizando el flujo de desarrollo, pruebas y despliegue de contratos inteligentes.

### **Directorios y Archivos Principales**

1. **`foundry.toml`**
    
    - **Función**: Archivo de configuración principal.
        
    - **Personalizaciones comunes**:
        
        - Versión del compilador de Solidity (`solc`).
            
        - Rutas de carpetas (`src`, `test`, `script`).
            
        - Remappings (mapeo de dependencias, como `@openzeppelin/=lib/openzeppelin-contracts/`).
            
        - Configuraciones de gas, optimizaciones, o plugins.
            
2. **`src/`**
    
    - **Contenido**: Contratos inteligentes (archivos `.sol`).
        
    - **Organización típica**:
        
        - Contratos principales en la raíz (ej: `Token.sol`).
            
        - Subcarpetas para funcionalidades modulares (ej: `src/utils/`, `src/interfaces/`).
            
    - **Convenciones**:
        
        - Nombre de archivos en PascalCase (ej: `MiContrato.sol`).
            
3. **`test/`**
    
    - **Contenido**: Pruebas escritas en Solidity (archivos `.t.sol`).
        
    - **Estructura**:
        
        - Cada archivo suele corresponder a un contrato probado (ej: `Token.t.sol`).
            
        - Herencia de `forge-std/Test.sol` para acceder a utilidades de prueba.
            
    - **Prácticas clave**:
        
        - Uso de `setUp()` para inicializar estados comunes.
            
        - Tests unitarios y de integración en el mismo lenguaje que los contratos.
            
4. **`script/`**
    
    - **Contenido**: Scripts de despliegue y automatización (archivos `.s.sol`).
        
    - **Uso típico**:
        
        - Desplegar contratos en redes locales (Anvil) o mainnets.
            
        - Configurar estados iniciales (ej: mintear tokens, asignar roles).
            
    - **Integración**:
        
        - Ejecutados con `forge script`, permitiendo simulaciones (`--dry-run`) o broadcasts reales.
            
5. **`lib/`**
    
    - **Contenido**: Dependencias externas (ej: OpenZeppelin, Solmate).
        
    - **Gestión**:
        
        - Instaladas via `forge install <github-repo>`.
            
        - Actualizadas con `forge update`.
            
    - **Característica única**:
        
        - Las dependencias son **submódulos de Git**, lo que garantiza transparencia y control.
            
6. **`out/`**
    
    - **Generado automáticamente**: Contiene los **artifacts de compilación** (ABI, bytecode, metadatos).
        
    - **Uso**:
        
        - Integración con frontends (ej: direcciones y ABI para interfaces web).
            
        - Análisis post-compilación (ej: verificación en Etherscan).
            
7. **`cache/`**
    
    - **Función**: Almacena caché de compilación para acelerar builds posteriores.
        
    - **Importante**: No se versiona en Git (se agrega a `.gitignore`).

### **Flujo de Trabajo Integrado**

1. **Desarrollo**:
    
    - Contratos se escriben en `src/`.
        
    - Dependencias se gestionan desde `lib/`.
        
2. **Pruebas**:
    
    - `forge test` ejecuta pruebas en `test/`, mostrando gas usado y resultados de fuzzing.
        
    - Entorno aislado: Las pruebas no afectan el estado de otros contratos.
        
3. **Despliegue**:
    
    - Scripts en `script/` se ejecutan con `forge script`, conectándose a redes locales (Anvil) o externas (Mainnet).
        
    - Configuración de redes y claves privadas manejada via variables de entorno o parámetros CLI.
        
4. **Optimización**:
    
    - Gas snapshots (`forge test --gas-report`) identifican funciones costosas.
        
    - Herramientas como `forge inspect` analizan artifacts en `out/`.