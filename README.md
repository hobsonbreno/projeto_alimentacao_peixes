# HydroSense - Sistema IoT de Monitoramento de Aquicultura

![HydroSense](https://img.shields.io/badge/HydroSense-v1.0-blue.svg)
![Platform](https://img.shields.io/badge/Platform-BitDogLab%20RP2040-green.svg)
![Language](https://img.shields.io/badge/Language-MicroPython-yellow.svg)

## 🐟 Sobre o Projeto

O **HydroSense** é um sistema IoT embarcado desenvolvido para monitorar e automatizar tanques de peixes e camarões, promovendo a sustentabilidade na aquicultura através de tecnologia acessível.

### Características Principais:
- 🌡️ **Monitoramento contínuo** de temperatura e pH
- 🤖 **Automação inteligente** de alimentação e troca de água
- 📱 **Dashboard web** para acompanhamento remoto
- 🔒 **Comunicação segura** via MQTT com TLS
- 💧 **Sustentabilidade** com reaproveitamento de água para irrigação

## 🛠️ Hardware Necessário

### Plataforma Base:
- **BitDogLab RP2040** (Raspberry Pi Pico W integrado)

### Sensores:
- **DS18B20** - Sensor de temperatura à prova d'água
- **Sensor de pH analógico** - Para monitoramento da acidez

### Atuadores:
- **Servo Motor SG90** - Dispenser automático de ração
- **Bomba submersa 12V** - Para troca de água
- **Módulo Relé** - Controle da bomba

### Periféricos (já inclusos na BitDogLab):
- **Display OLED 128x64** - Interface local
- **Matriz LEDs RGB 5x5** - Indicadores visuais
- **Buzzer** - Alertas sonoros
- **Botões A e B** - Controle manual

## 🔧 Instalação e Configuração

### 1. Preparação do Hardware
```
1. Conecte o DS18B20 ao GPIO18 (OneWire)
2. Conecte o sensor pH ao GPIO26 (ADC)
3. Conecte o servo motor ao GPIO16 (PWM)
4. Conecte o relé da bomba ao GPIO17
5. Verifique as conexões da BitDogLab (OLED, LEDs, buzzer)
```

### 2. Instalação do Software
```bash
# 1. Instale o firmware BitDogLab_W.uf2 no Pico W
# 2. Copie os arquivos para a placa via Thonny:
- hydrosense_main.py (como main.py)
- hydrosense_config.py
- ssd1306.py (biblioteca OLED)
```

### 3. Configuração de Rede
Edite o arquivo `hydrosense_config.py`:
```python
class NetworkConfig:
    WIFI_SSID = "SUA_REDE_WIFI"
    WIFI_PASSWORD = "SUA_SENHA_WIFI"
    MQTT_BROKER = "seu-broker-mqtt.com"
```

### 4. Parâmetros de Aquicultura
Ajuste conforme sua espécie de peixe:
```python
class AquacultureParams:
    TEMP_MIN = 24.0  # °C mínima
    TEMP_MAX = 28.0  # °C máxima
    PH_MIN = 6.5     # pH mínimo
    PH_MAX = 8.0     # pH máximo
    FEED_INTERVAL = 8 * 3600  # Alimentação a cada 8h
```

## 📊 Dashboard e Monitoramento

### Tópicos MQTT:
- `hydrosense/temperatura` - Dados de temperatura
- `hydrosense/ph` - Dados de pH
- `hydrosense/status` - Status geral do sistema
- `hydrosense/alertas` - Alertas críticos
- `hydrosense/comandos` - Comandos remotos

### Comandos Remotos via MQTT:
```json
// Alimentação manual
{"action": "feed"}

// Troca de água (30 segundos)
{"action": "pump", "duration": 30}

// Solicitar status
{"action": "status"}
```

## 🎮 Interface Local (BitDogLab)

### Display OLED:
- Temperatura atual
- pH atual
- Status WiFi/MQTT
- Contagem de alertas
- Timer de alimentação

### LEDs de Status:
- **LED Central (12)**: Status geral do sistema
  - 🟢 Verde = Parâmetros OK
  - 🟡 Amarelo = Atenção
  - 🔴 Vermelho = Crítico

- **LED Esquerdo (11)**: Status temperatura
- **LED Direito (13)**: Status pH
- **LED Superior (24)**: Status WiFi

### Controles Manuais:
- **Botão A**: Alimentação manual
- **Botão B**: Troca de água manual

## 🏗️ Arquitetura do Sistema

### Tasks Principais (Conceito FreeRTOS):
1. **Sensor Task** - Leitura contínua dos sensores
2. **Control Task** - Automação e controles manuais
3. **Communication Task** - WiFi, MQTT e interface

### Fluxo de Dados:
```
Sensores → Processamento → Alertas → MQTT → Dashboard
    ↓           ↓            ↓        ↓
  Display → Controle → Atuadores → Logs
```

## 📈 Parâmetros Ideais por Espécie

### Tilápia:
- **Temperatura**: 26-30°C
- **pH**: 6.5-8.5
- **Alimentação**: 3-4x/dia

### Tambaqui:
- **Temperatura**: 24-28°C
- **pH**: 6.0-7.5
- **Alimentação**: 2-3x/dia

### Camarão (Litopenaeus vannamei):
- **Temperatura**: 28-32°C
- **pH**: 7.5-8.5
- **Alimentação**: 4-6x/dia

## 🔒 Segurança

### Implementações de Segurança:
- **Autenticação MQTT** com usuário/senha
- **Validação de comandos** remotos
- **Timeouts de conexão** para robustez
- **Watchdog timer** para recuperação automática

### Recomendações:
- Use brokers MQTT com TLS em produção
- Configure senhas fortes
- Monitore logs de acesso
- Mantenha firmware atualizado

## 🌱 Sustentabilidade

### Funcionalidades Ecológicas:
- **Reaproveitamento de água** para irrigação de hortas
- **Otimização da alimentação** reduz desperdício
- **Monitoramento preciso** evita uso excessivo de recursos
- **Alertas preventivos** evitam perdas de animais

## 📋 Cronograma de Desenvolvimento

### Fase 1 (Semanas 1-2): Prototipagem
- [x] Definição de componentes
- [x] Montagem do circuito
- [x] Testes básicos de sensores

### Fase 2 (Semanas 3-4): Implementação Core
- [x] Leitura de sensores DS18B20 e pH
- [x] Interface OLED e LEDs
- [x] Controles manuais

### Fase 3 (Semanas 5-6): IoT e Automação
- [x] Conectividade WiFi
- [x] Protocolo MQTT
- [x] Automação de alimentação/bomba
- [x] Sistema de alertas

### Fase 4 (Semana 7): Integração e Testes
- [x] Testes integrados
- [x] Dashboard web
- [x] Documentação final
- [x] Vídeo demonstrativo

## 🎯 Resultados Esperados

### Técnicos:
- ✅ Monitoramento 24/7 de parâmetros aquícolas
- ✅ Automação de rotinas críticas
- ✅ Comunicação IoT robusta e segura
- ✅ Interface intuitiva local e remota

### Ambientais:
- 🌍 Redução do desperdício de ração (até 20%)
- 💧 Reaproveitamento de água para irrigação
- 📊 Otimização do uso de recursos naturais
- 🐟 Melhoria no bem-estar animal

## 🚀 Evolução Futura

### Funcionalidades Planejadas:
- **Múltiplos tanques** - Suporte a vários pontos
- **IA/ML** - Predição de padrões e otimização
- **App mobile** - Interface nativa para smartphone
- **Sensores adicionais** - Oxigênio dissolvido, turbidez
- **Integração com ERPs** - Gestão comercial completa

## 📚 Bibliografia e Referências

1. Tolomelli, J. (2024). *Repositório de drivers Raspberry Pi Pico W*. GitHub.
2. Bouguettaya, A., & Luo, X. (2019). *Internet of Things: Principles and Paradigms*. Springer.
3. Kamath, M., & Padhy, R. (2020). *IoT-based water quality monitoring system for aquaculture*. IJEA.
4. FreeRTOS Documentation (2025). https://www.freertos.org
5. MQTT Protocol Specification (2025). https://mqtt.org
6. BitDogLab RP2040 Official Documentation (2025). https://bitdoglab.com/docs/rp2040

## 📞 Suporte e Contribuições

### Contato:
- **Projeto**: HydroSense IoT Aquaculture
- **Plataforma**: BitDogLab RP2040
- **Licença**: MIT License

### Como Contribuir:
1. Fork o repositório
2. Crie uma branch para sua feature
3. Implemente suas melhorias
4. Teste extensivamente
5. Submeta um Pull Request

---

**Desenvolvido com 💙 para a sustentabilidade na aquicultura**
