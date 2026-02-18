# Taller Práctico de Ansible para HPC

## Estructura de Archivos

```text
.
├── inventory.ini
├── ansible.cfg
├── playbooks/
│   ├── health_check.yml
│   ├── install_package.yml
│   ├── template_empty.yml
│   └── scientific_stack.yml
├── files/
│   └── welcome.sh

```

- `inventory.ini`: inventario del sandbox con `login_nodes` y `compute_nodes`.
- `ansible.cfg`: configuración base de Ansible para el taller.
- `playbooks/`: ejercicios prácticos (1 al 4).
- `files/`: archivos auxiliares para prácticas.
- `solutions/`: soluciones de referencia para revisión posterior.

## Verificación Inicial

Ejecuta conectividad básica desde el directorio del taller:

```bash
ansible all -i inventory.ini -m ping
```

## Ejercicios

- [Ejercicio 1 - Health Check](playbooks/health_check.yml)
- [Ejercicio 2 - Install Package](playbooks/install_package.yml)
- [Ejercicio 3 - Plantilla Base](playbooks/template_empty.yml)
- [Ejercicio 4 - Scientific Stack](playbooks/scientific_stack.yml)

## Comandos Útiles

Cheat sheet (Slide 23):

| Propósito | Comando | Cuándo usarlo |
|---|---|---|
| Validar sintaxis YAML | `ansible-playbook playbook.yml --syntax-check` | Antes de ejecutar cualquier playbook nuevo o modificado |
| Dry-run (simular ejecución) | `ansible-playbook -i inventory.ini playbook.yml --check` | Para ver qué cambiaría sin aplicar cambios reales |
| Ver información detallada | `ansible-playbook -i inventory.ini playbook.yml -v` | Para debugging o entender qué está haciendo Ansible |
| Ejecutar en un solo nodo | `ansible-playbook -i inventory.ini playbook.yml --limit node01` | Para probar en un nodo específico antes del despliegue masivo |
| Ver información del sistema | `ansible all -i inventory.ini -m setup -a "filter=ansible_distribution*"` | Para conocer detalles de OS, versiones, hardware de los nodos |
| Ejecutar comando simple | `ansible compute_nodes -i inventory.ini -a "uptime"` | Para diagnóstico rápido sin crear playbook |

Tip: usa `-vv` o `-vvv` para más verbosidad durante troubleshooting.

## Troubleshooting

Tabla de errores frecuentes (Slide 24):

| Error | Causa | Solución |
|---|---|---|
| `ERROR! Syntax Error while loading YAML` | Indentación incorrecta o caracteres especiales mal escapados | Usa un editor con validación YAML. Verifica que uses espacios (no tabs). |
| `Permission denied o Missing sudo password` | La tarea requiere privilegios root pero no se especificó | Agrega `become: yes` al playbook o a la tarea específica. |
| `Host unreachable o Failed to connect to the host` | Problemas de red, SSH, o inventario incorrecto | Verifica conectividad con `ansible host -m ping`. Revisa IP en `inventory.ini`. |
| `The module <nombre> was not found` | Nombre del módulo escrito incorrectamente | Consulta documentación oficial: `docs.ansible.com/modules`. Verifica mayúsculas. |
| `changed: false cuando esperabas cambios` | El estado deseado ya existe (idempotencia funcionando) | Es normal: el nodo ya está en el estado correcto. |
| `Variables no se expanden {{ variable }}` | Variables no definidas o scope incorrecto | Verifica que la variable esté en `vars:` o en el inventario. Usa `-vv` para debug. |
