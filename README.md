# Wearable Device for Real-Time Sign Language Data Acquisition

A wearable device that collects real-time gesture data using flex sensors and an STM32 Nucleo microcontroller. The device transmits the collected data via USB, allowing users to process and use the data for their specific applications.

## Table of Contents
- [Project Overview](#project-overview)
- [Features](#features)
- [Hardware Components](#hardware-components)
- [How It Works](#how-it-works)
- [Getting Started](#getting-started)
- [Data Usage](#data-usage)
- [Troubleshooting](#troubleshooting)
- [Acknowledgments](#acknowledgments)

---

## Project Overview

This project collects real-time gesture data based on finger movements using flex sensors. The data is sent from the STM32 microcontroller to a host system via USB in a simple, easy-to-parse format.

### Use Cases:
- Gesture classification for sign language recognition.
- Input for machine learning models.
- Real-time visualization or logging of gesture data.

---

## Features

- **Real-Time Data Acquisition**: Flex sensor data is read and transmitted instantly.
- **USB Communication**: Transmits data as a virtual COM port for easy integration with host systems.
- **Customizable**: The host can use the data for various purposes, such as machine learning or visualization.

---

## Hardware Components

- **Flex Sensors**: Mounted on three fingers to detect bending angles.
- **STM32 Nucleo Microcontroller**: Processes sensor data and transmits it to the host.
- **USB Cable**: For data transmission and power supply.
- **Additional Components**: Resistors for voltage divider circuits.

---

## How It Works

1. **Data Collection**:
   - Flex sensors measure the bending of fingers and output an analog voltage.
   - The STM32's ADC reads the voltage values from three sensors.

2. **Data Formatting**:
   - The microcontroller converts the analog values into a comma-separated string, e.g., `1023,850,600\n`.

3. **Data Transmission**:
   - The formatted data is sent to the host system over USB using the USB CDC (virtual COM port) protocol.

---

## Getting Started

### Prerequisites

- STM32CubeIDE for programming the STM32 microcontroller.
- USB CDC driver installed on your host system to recognize the microcontroller as a virtual COM port.
- Serial communication software or a custom script to read the data from the COM port.

### Steps

1. **Set Up the Device**:
   - Connect the flex sensors to the STM32 Nucleo's ADC pins.
   - Power the STM32 via USB.

2. **Install USB CDC Driver**:
   - Ensure the appropriate USB CDC driver is installed on your host system (often automatic on modern operating systems).

3. **Run Serial Communication Software**:
   - Use software like [Tera Term](https://ttssh2.osdn.jp/index.html.en), [PuTTY](https://www.putty.org/), or a Python script to read data from the COM port.

4. **Retrieve Data**:
   - Configure the serial communication settings:
     - **Baud Rate**: Ignored for USB CDC, but set a default (e.g., 9600).
     - **Data Bits**: 8.
     - **Parity**: None.
     - **Stop Bits**: 1.
   - Connect to the virtual COM port created by the STM32.
   - Read the streamed data in real-time.

---

## Data Usage

The microcontroller outputs data in the following format:
```
<adc_val1>,<adc_val2>,<adc_val3>\n
```

Example:
```
1023,850,600
1012,845,595
1008,840,590
```

### Using the Data in Python
You can use Python to capture and process the data. Here's an example script:

```python
import serial

# Replace 'COMx' with the appropriate port (e.g., 'COM3' on Windows, '/dev/ttyUSB0' on Linux)
ser = serial.Serial('COM3', baudrate=9600, timeout=1)

while True:
    # Read a line of data
    data = ser.readline().decode('utf-8').strip()
    
    if data:
        # Split the data into individual sensor values
        adc_values = data.split(',')
        print(f"Received ADC Values: {adc_values}")

        # Example: Convert to integers for processing
        adc_values = list(map(int, adc_values))
```

### Applications
- **Machine Learning**: Feed the data into a classification model.
- **Visualization**: Plot the data using libraries like `matplotlib` for real-time analysis.
- **Logging**: Save the data to a file for later analysis.

---

## Troubleshooting

1. **No Data Received**:
   - Ensure the USB CDC driver is installed correctly.
   - Check the COM port in the device manager (Windows) or with `ls /dev/tty*` (Linux).

2. **Garbled Data**:
   - Verify the serial communication settings (8-N-1 format).
   - Ensure the USB cable is not faulty.

3. **Microcontroller Not Recognized**:
   - Reinstall the USB CDC driver.
   - Confirm the STM32 firmware is programmed correctly.

---

## Acknowledgments

This project was developed as part of the *ES 333 Microprocessors and Embedded Systems* course at IIT Gandhinagar. Special thanks to the faculty for their guidance and support.

---

## License

This project is licensed under the [MIT License](LICENSE). You are free to modify and use the code for non-commercial purposes.
