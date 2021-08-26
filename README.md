# Home Assistant Sprinkler Controller
A remotely-accessible sprinkler controller built using [Home Assistant](https://www.home-assistant.io/) and Raspberry Pi 3B+
![Sprinkler Controller Scheduler](https://user-images.githubusercontent.com/24600116/130844032-bcc18142-818a-445a-b8c6-75ed0e62ba6b.png)
# Design Goals
I wanted a fairly simple sprinkler controller that could handle up to 8 valves and run sequential schedules at certain times of the day and for certain days of the week. I run Home Assistant for general home automation and wanted to integrate the sprinkler scheduling into my existing setup.
# Features
* Remote/mobile access via [Home Assistant mobile app](https://www.home-assistant.io/integrations/mobile_app/) and [remote access](https://www.home-assistant.io/docs/configuration/remote/)
* Multiple schedules that can be toggled on/off with configurable runtime for each valve
* Ability to delay or schedule off-time for all schedules to account for precipitation
* Manual control of each sprinkler valve for inspection/calibration
* Enforce only one valve open at a time; I don't have the water pressure to run multiple valves at once and most controllers only allow for one to run at a time
# Softare Requirements
* [32-bit Home Assistant OS for Raspberry Pi 3B+](https://github.com/home-assistant/operating-system/releases/download/6.2/haos_rpi4-6.2.img.xz)
* [Date-time sensor integration](https://www.home-assistant.io/integrations/time_date/)
# Hardware Requirements
* [Raspberry Pi 3B+ with power source](https://www.amazon.com/ELEMENT-Element14-Raspberry-Pi-Motherboard/dp/B07P4LSDYV/ref=sr_1_3?dchild=1&keywords=raspberry+pi+3b%2B&qid=1630003037&s=electronics&sr=1-3)
* [BUD Industries electric/utility box](https://www.amazon.com/gp/product/B005UPANU2/ref=ppx_yo_dt_b_asin_title_o02_s00?ie=UTF8&psc=1)
* [Plastic internal panel](https://www.amazon.com/gp/product/B005UPE83U/ref=ppx_yo_dt_b_asin_title_o02_s00?ie=UTF8&psc=1) to secure parts
* [24VAC to 5V DC converter](https://www.amazon.com/gp/product/B00RE6QN4U/ref=ppx_yo_dt_b_asin_title_o01_s00?ie=UTF8&psc=1) to power the relay
* [Jumper wires](https://www.amazon.com/gp/product/B01LZF1ZSZ/ref=ppx_yo_dt_b_asin_title_o03_s00?ie=UTF8&psc=1)
* [5V 8 relay board](https://www.amazon.com/gp/product/B00KTELP3I/ref=ppx_yo_dt_b_asin_title_o03_s00?ie=UTF8&psc=1)
* [24VA 1.6A transformer](https://www.amazon.com/gp/product/B07QS5JPKZ/ref=ppx_yo_dt_b_asin_title_o03_s02?ie=UTF8&psc=1)
# Wiring Diagram
This is a slightly modified design from other common Raspberry Pi-based sprinkler controllers. Instead of powering the relay off of the Pi board, an AC to 5V DC buck converter is wired to the 24VAC transformer to power the relay via the JCC-VD pin (jumper removed). This is to ensure the relay does draw current directly from the Pi board, which may not be enough to power it.
![Sprinkler Wiring Diagram 4](https://user-images.githubusercontent.com/24600116/131020111-73a72e33-26a6-4594-93d6-506eff3990de.png)
# Code
The code includes two files. The sprinklers.yaml file contains all of the scheduling code and is loaded as a [package](https://www.home-assistant.io/docs/configuration/packages/) via the Home Assistant config file. The Sprinkler Scheduler View.yaml file contains code the can be pasted into the raw view of the LoveLace frontend editor. This will provide a single view for calibrating the sprinkler system as well as schedule delays and determining when the system will run next.
# Other Notes
* If Home Assistant or the Raspberry Pi shuts down, the sprinkler valves will close so there is no chance the sprinkler system will stay on in case of a hardware or software failure (barring some kind of mechanical problem with the sprinkler valves themselves).
* If Home Assistant reboots or the schedule is toggled off/on, the code will resume a running schedule and calculate the remaining runtime for the scheduled zone.
# Final Build
My Home Assistant Raspberry Pi lives in my garage where it also runs a Z-wave network along with some other integrations. The Pi connects to my home network using an ethernet drop that runs to a POE switch. I use a [POE hat](https://www.amazon.com/LoveRPi-Power-Over-Ethernet-Raspberry-Model-Translucent/dp/B07KF7F9GS?pd_rd_w=E6rIk&pf_rd_p=ba22ea03-d3c1-4ebb-8d3e-472aa0cf3b30&pf_rd_r=FK1E981C4ZG60GS2P50M&pd_rd_r=282f1537-356c-44a4-aaec-7af5d6242464&pd_rd_wg=cPTu2&pd_rd_i=B07KF7F9GS&psc=1&ref_=pd_bap_d_rp_2_t) to provide power to the Pi.
![0825211128a](https://user-images.githubusercontent.com/24600116/131020877-b00292a4-a38e-4e3c-9e05-7b6b0d7b8c4d.jpg)
# TODO
* Weather/precipitation integration. I intially tried to automate this based on recorded precipitation from one of the weather API integrations in Home Assistant. However I found it easiest to just manually set a schedule delay after checking the forecast. With a traditional controller I would often forget to turn the sprinklers back on after turning them off for rainy days and this was enough to overcome this problem. However I may try to incorporate a rain sensor into the setup at a later time.
* Schedule that runs every N days instead of certain days of the week
