# Automated Checkout System (AutoBill)

An intelligent automated checkout system that uses computer vision and weight sensors to identify and bill products automatically. This system combines machine learning for product recognition with load cell sensors for weight measurement to create a seamless shopping experience.

## 🌟 Features

- **Automatic Product Recognition**: Uses Edge Impulse machine learning model for real-time product identification
- **Weight-based Validation**: HX711 load cell integration for accurate weight measurement
- **Real-time Billing**: Automatic price calculation based on detected products and weights
- **Web Dashboard**: Clean web interface for checkout management
- **RESTful API**: Backend API for product and order management
- **QR Code Payment**: Integrated payment system with QR code generation

## 🛠️ Hardware Requirements

- Raspberry Pi (with GPIO access)
- Camera module or USB webcam
- HX711 Load Cell Amplifier
- Load cell/weight sensor
- Display screen (for web interface)

### Hardware Connections

| Component | GPIO Pin |
|-----------|----------|
| HX711 DOUT | GPIO 20 |
| HX711 PD_SCK | GPIO 21 |

## 📋 Software Requirements

### Python Dependencies
```bash
pip install opencv-python
pip install RPi.GPIO
pip install requests
pip install edge-impulse-linux
```

### Node.js Dependencies
```bash
cd CheckoutUI/server
npm install
```

## 🚀 Installation & Setup

### 1. Clone the Repository
```bash
git clone https://github.com/hardbuilder/Automated-Checkout-System.git
cd Automated-Checkout-System
```

### 2. Hardware Calibration
Run the calibration script to set up your load cell:
```bash
python3 calibration.py
```
Follow the on-screen instructions to calibrate with a known weight.

### 3. Machine Learning Model
Ensure you have your trained Edge Impulse model file (`modelfile.eim`) in the project directory.

### 4. Start the Backend Server
```bash
cd CheckoutUI/server
npm start
```
The server will run on port 3000 (or PORT environment variable).

### 5. Launch the Main Application
```bash
python3 billing.py modelfile.eim
```
Optionally specify camera port:
```bash
python3 billing.py modelfile.eim 0
```

## 🏗️ System Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Camera Feed   │────│  ML Recognition │────│   Product ID    │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                                        │
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  Load Cell      │────│  Weight Sensor  │────│   Weight Data   │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                                        │
                               ┌─────────────────┐     │
                               │  Billing Logic │◄────┘
                               └─────────────────┘
                                        │
                               ┌─────────────────┐
                               │   Web API      │
                               └─────────────────┘
                                        │
                               ┌─────────────────┐
                               │  Web Interface │
                               └─────────────────┘
```

## 📱 Web Interface

The system includes a responsive web interface accessible at `http://localhost:3000` with:

- Real-time product display
- Automatic bill calculation
- QR code payment integration
- Order management

## 🔧 Configuration

### Supported Products
Currently configured for:
- **Apple** - ₹10/kg (Rate: 0.01 per gram)
- **Banana** - ₹20/kg (Rate: 0.02 per gram) 
- **Lays** - ₹1 per packet
- **Coke** - ₹2 per bottle

### API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | API status |
| POST | `/product` | Add new product |
| GET | `/product` | Get all products |
| GET | `/product/:id` | Get specific product |
| DELETE | `/product/:id` | Delete product |
| POST | `/checkout` | Process checkout |
| GET | `/checkout` | Get all orders |

## 🎯 How It Works

1. **Product Detection**: Camera captures images and ML model identifies products
2. **Weight Measurement**: Load cell measures weight changes
3. **Validation**: System correlates detected products with weight changes
4. **Billing**: Automatic price calculation based on product type and weight
5. **Checkout**: Web interface displays bill and generates QR code for payment

## 🔄 Workflow

```
Place Item → Camera Detection → Weight Measurement → Product Validation → Add to Bill → Checkout → Payment
```

## ⚙️ Customization

### Adding New Products
1. Train your Edge Impulse model with new product images
2. Update the product recognition logic in `billing.py`
3. Add pricing information in the `rate()` function

### Adjusting Sensitivity
- Modify detection threshold (currently 0.9) in line 214 of `billing.py`
- Adjust weight sensitivity threshold in `list_com()` function

## 🐛 Troubleshooting

### Common Issues

**Camera not detected:**
```bash
# Check available cameras
python3 -c "import cv2; print([i for i in range(5) if cv2.VideoCapture(i).isOpened()])"
```

**Weight sensor not working:**
- Check GPIO connections
- Run calibration script
- Verify HX711 wiring

**Model not loading:**
- Ensure `modelfile.eim` exists
- Check Edge Impulse Linux SDK installation

## 📁 File Structure

```
Automated-Checkout-System/
├── billing.py              # Main application logic
├── calibration.py          # Load cell calibration
├── hx711.py               # HX711 sensor driver
├── modelfile.eim          # ML model file
├── CheckoutUI/
│   ├── client/
│   │   ├── index.html     # Web interface
│   │   ├── add.html       # Admin interface
│   │   └── assets/
│   │       └── css/
│   │           └── style.css
│   ├── server/
│   │   ├── server.js      # Express.js backend
│   │   ├── package.json   # Dependencies
│   │   └── package-lock.json
│   ├── script.js          # Frontend JavaScript
│   └── *.jpg             # UI screenshots
└── README.md
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## 📄 License

This project is licensed under the ISC License.

## 👨‍💻 Author

**Shebin Jose Jacob**

## 🔗 Links

- [Edge Impulse](https://edgeimpulse.com/) - For ML model training
- [Raspberry Pi GPIO](https://www.raspberrypi.org/documentation/usage/gpio/) - GPIO documentation



⚡ **Ready to revolutionize retail checkout!** ⚡
