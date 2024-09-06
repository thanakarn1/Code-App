<template>
  <br />
  <v-responsive max-width="100%">
    <div class="container">
      <v-row class="text-center">
        <!-- Temperature Card -->
        <v-col cols="12" sm="12" md="12">
          <v-card
            class="mx-auto"
            max-width="1000"
            hover
            style="
              backdrop-filter: blur(10px);
              width: 100%;
              margin: 0;
              text-align: center;
              padding: 0;
              color: white;
              background: linear-gradient(#141e30, #243b52);
              background: transparent;
              border: 2px solid rgba(255, 255, 255, 0.2);
              border-radius: 30px;
              box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
            "
          >
            <!-- ส่วนหัวสถานะเปิด ปิด ใช้งาน -->
            <v-card-text>
              <v-row>
                <v-col cols="12">
                  <v-row class="d-flex align-center justify-center">
                    <v-col cols="6">
                      <br /><br />
                      <div
                        class="rounded-circle mx-auto"
                        :style="{
                          height: '32px',
                          width: '32px',
                          background: isOn ? '#43ff19' : '#EEE',
                        }"
                      ></div>
                      ON
                    </v-col>
                    <v-col cols="6">
                      <br /><br />
                      <div
                        class="rounded-circle mx-auto"
                        :style="{
                          height: '32px',
                          width: '32px',
                          background: !isOn ? 'red' : '#EEE',
                        }"
                      ></div>
                      OFF
                    </v-col>
                  </v-row>
                  <!-- สิ้นสุดส่วนสถานะ เปิดปิด -->

                  <!-- ค่าอุณหภูมิจากตัวแปร -->
                  <v-row>
                    <v-col cols="6">
                      <h2>อุณหภูมิ</h2>
                      <h4 style="color: red">{{ value_temp }} C</h4>
                      <h2>{{ systemStatus }}</h2>
                    </v-col>
                    <v-col cols="6">
                      <h2>ความชื้น</h2>
                      <h4 style="color: blue">{{ value_humi }} PH</h4>
                      <h2>{{ systemStatus1 }}</h2>
                    </v-col>
                  </v-row>
                  <!-- สิ้นสุด ค่าอุณหภูมิจากตัวแปร -->

                  <!-- สถานะที่ถูกเปิดใช้งาน -->
                  <br />
                  <h1>สถานะการที่ {{ value_state }}</h1>
                  <v-progress-linear
                    color="deep-orange"
                    height="20"
                    :model-value="value_temp"
                    striped
                  ></v-progress-linear>
                  <v-progress-linear
                    color="light-blue"
                    height="20"
                    :model-value="value_humi"
                    striped
                  ></v-progress-linear>
                  ผลรวม
                  <v-progress-linear
                    color="lime"
                    height="20"
                    :model-value="totalValue"
                    striped
                  ></v-progress-linear>
                </v-col>
              </v-row>

              <!-- สิ้นสุด สถานะที่ถูกเปิดใช้งาน -->

              <!-- คำสั่งเปิด และ ปิด การทำงาน -->
              <v-row>
                <v-col cols="12" sm="6">
                  <v-btn
                    block
                    style="background-color: #34f6d6; color: #fff; height: 50px"
                    @click="start"
                    >เปิด</v-btn
                  >
                </v-col>
                <br />
                <v-col cols="12" sm="6">
                  <v-btn
                    block
                    style="background-color: #f999ff; color: #fff; height: 50px"
                    @click="stop"
                    >ปิด</v-btn
                  >
                </v-col>
              </v-row>
            </v-card-text>
            <!-- สิ้นสุด คำสั่งเปิด และ ปิด การทำงาน -->
            <v-card
              class="mx-auto"
              max-width="400"
              hover
              style="
                backdrop-filter: blur(10px);
                width: 100%;
                margin: 0;
                text-align: start;
                padding: 0;
                color: white;
                background: linear-gradient(#141e30, #243b52);
                background: transparent;
                border: 2px solid rgba(255, 255, 255, 0.2);
                border-radius: 30px;
                box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
                margin-bottom: 20px;
              "
            >
              <v-card-text>
                <v-row>
                  <!-- ปุ่มTest อุณหภูมิตามสถานะการณ์ -->
                  <v-col cols="12">
                    <v-btn
                      @click="cycleLights"
                      class="mt-2"
                      :style="{ backgroundColor: getButtonColor(index) }"
                      block
                      :disabled="!isOn"
                    >
                      {{ stepButtonLabel }}
                    </v-btn>
                  </v-col>
                </v-row>
              </v-card-text>
            </v-card>
          </v-card>
        </v-col>

        <!-- สิ้นสุด ปุ่มTest อุณหภูมิตามสถานะการณ์ -->

        <v-col cols="12">
          <v-card
            class="mx-auto"
            max-width="1000"
            hover
            style="
              backdrop-filter: blur(10px);
              width: 100%;
              margin: 0;
              text-align: center;
              padding: 0;
              color: white;
              background: linear-gradient(#141e30, #243b52);
              background: transparent;
              border: 2px solid rgba(255, 255, 255, 0.2);
              border-radius: 30px;
              box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
            "
          >
            <v-card-title>
              <v-col cols="12">
                <h2>อุณหภูมิและความชื้น</h2>
              </v-col>
            </v-card-title>
            <v-card-text>
              <v-row>
                <v-col cols="3" sm="3" md="3">
                  <v-row>
                    <v-col cols="12" class="text-center">
                      <h2>อุณหภูมิ</h2>
                    </v-col>
                  </v-row>
                  <!-- อุณหภูมิ วงกลม 1 -->
                  <v-progress-circular
                    :model-value="value_temp"
                    :rotate="360"
                    :size="180"
                    :width="34"
                    color="red"
                  >
                    <template v-slot:default> {{ value_temp }} °C </template>
                  </v-progress-circular>
                </v-col>

                <!-- ความชื้น วงกลม 2 -->
                <v-col cols="3" sm="3" md="3">
                  <v-row>
                    <v-col cols="12" class="text-center">
                      <h2>ความชื้น</h2>
                    </v-col>
                  </v-row>
                  <v-progress-circular
                    :model-value="value_humi"
                    :rotate="360"
                    :size="180"
                    :width="34"
                    color="blue"
                  >
                    <template v-slot:default> {{ value_humi }} % </template>
                  </v-progress-circular>
                </v-col>
                <!-- อุณหภูมิที่เหมาะสม วงกลม 3 -->
                <v-col cols="3" sm="3" md="3">
                  <v-row>
                    <v-col cols="12" class="text-center">
                      <h2>อุณหภูมิที่เหมาะสม</h2>
                    </v-col>
                  </v-row>
                  <v-progress-circular
                    :model-value="totalValueT"
                    :rotate="360"
                    :size="180"
                    :width="34"
                    color="red"
                  >
                    {{ totalValueT }} °C
                  </v-progress-circular>
                </v-col>

                <!-- ความชื้นที่เหมาะสม วงกลม 4 -->
                <v-col cols="3" sm="3" md="3">
                  <v-row>
                    <v-col cols="12" class="text-center">
                      <h2>ความชื้นที่เหมาะสม</h2>
                    </v-col>
                  </v-row>
                  <v-progress-circular
                    :model-value="totalValueH"
                    :rotate="360"
                    :size="180"
                    :width="34"
                    color="blue"
                  >
                    {{ totalValueH }} %
                  </v-progress-circular>
                </v-col>
                <!-- สิ้นสุดสถานะวงกลม -->

                <!-- ค่าปกติ -->
                <v-row>
                  <v-col cols="3">
                    <div>ต่ำสุด</div>
                    <div>{{ value_t1 }} °C</div>
                  </v-col>
                  <v-col cols="3">
                    <div>ต่ำสุด</div>
                    <div>{{ value_h1 }} %</div>
                  </v-col>

                  <v-col cols="3">
                    <div>ไม่เกิน</div>
                    <div>{{ value_t2 }} °C</div>
                  </v-col>
                  <v-col cols="3">
                    <div>ไม่เกิน</div>
                    <div>{{ value_h2 }} %</div>
                  </v-col>
                </v-row>
              </v-row>
            </v-card-text>

            <!-- สิ้นสุดค่าปกติ -->

            <v-card
              class="mx-auto"
              max-width="700"
              hover
              style="
                backdrop-filter: blur(10px);
                width: 100%;
                margin: 0;
                text-align: center;
                padding: 0;
                color: white;
                background: linear-gradient(#141e30, #243b52);
                background: transparent;
                border: 2px solid rgba(255, 255, 255, 0.2);
                border-radius: 30px;
                box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
                margin-bottom: 20px;
              "
            >
              <v-card-text>
                <v-row>
                  <v-col cols="12">
                    <v-row class="d-flex align-center justify-center">
                      <v-col cols="4">
                        <!-- ค่าแสดงสถานะ ความร้อน -->
                        <div
                          class="rounded-circle mx-auto"
                          :style="{
                            height: '32px',
                            width: '32px',
                            backgroundColor: lightStates[0] ? 'red' : '#ddd',
                          }"
                        ></div>
                        <div>ความร้อน</div>
                      </v-col>

                      <!-- ค่าแสดงสถานะ ปกติ -->
                      <v-col cols="4">
                        <div
                          class="rounded-circle mx-auto"
                          :style="{
                            height: '32px',
                            width: '32px',
                            backgroundColor: lightStates[1] ? 'green' : '#ddd',
                          }"
                        ></div>
                        <div>ปกติ</div>
                      </v-col>

                      <!-- ค่าแสดงสถานะ เย็น -->
                      <v-col cols="4">
                        <div
                          class="rounded-circle mx-auto"
                          :style="{
                            height: '32px',
                            width: '32px',
                            backgroundColor: lightStates[2] ? 'blue' : '#ddd',
                          }"
                        ></div>
                        <div>เย็น</div>
                      </v-col>
                    </v-row>

                    <!-- สถานะของอุณหภูมิ และ ความชื้น -->
                    <h1>{{ systemStatus }}</h1>
                  </v-col>
                </v-row>
              </v-card-text>
            </v-card>
          </v-card>
        </v-col>
      </v-row>

      <br />
    </div>
    <!-- </v-parallax> -->
  </v-responsive>
</template>
<script>
// ส่วนนี้คือการกำหนดค่าตั้งต้นที่ใช้ในคอมโพเนนต์ ตัวแปรต่าง ๆ ที่ระบุจะเก็บข้อมูลเกี่ยวกับสถานะต่าง ๆ เช่น อุณหภูมิ ความชื้น สถานะของไฟ LED เป็นต้น.
import mqtt from "mqtt"; // นำเข้าไลบรารี MQTT สำหรับการสื่อสาร

export default {
  data() {
    return {
      // ตัวแปรสถานะสำหรับค่าต่าง ๆ และสถานะของระบบ
      value: 0,
      value1: 0,
      originalValue: 0,
      originalValue1: 0,
      isOn: false,
      value_temp: 0, // ค่าปัจจุบันของอุณหภูมิ
      value_humi: 0, // ค่าปัจจุบันของความชื้น
      value_state: "", // สถานะปัจจุบันของระบบ
      value_t1: 0, // ขีดจำกัดอุณหภูมิที่ 1
      value_t2: 0, // ขีดจำกัดอุณหภูมิที่ 2
      value_h1: 0, // ขีดจำกัดความชื้นที่ 1
      value_h2: 0, // ขีดจำกัดความชื้นที่ 2
      isOnlight: false, // สถานะของระบบไฟ
      lightStates: [false, false, false], // อาเรย์เก็บสถานะของไฟแต่ละดวง
      stepIndex: 0, // ใช้ติดตามขั้นตอนปัจจุบัน
      statustem: "", // สถานะของระบบอุณหภูมิ
      systemStatus: "", // สถานะโดยรวมของระบบ
      systemStatus1: "", // สถานะของระบบเพิ่มเติม
    };
  },

  computed: {
    // คอมพิวเตดพรอพเพอร์ตี้เพื่อคำนวณค่ารวมจากอุณหภูมิและความชื้น
    totalValue() {
      const ttotal = Number(this.value_temp) || 0;
      const htotal = Number(this.value_humi) || 0;
      return ttotal + htotal;
    },

    // คอมพิวเตดพรอพเพอร์ตี้เพื่อคำนวณค่าเฉลี่ยของอุณหภูมิ
    totalValueT() {
      const t1 = Number(this.value_t1) || 0;
      const t2 = Number(this.value_t2) || 0;
      return (t1 + t2) / 2;
    },

    // คอมพิวเตดพรอพเพอร์ตี้เพื่อคำนวณค่าเฉลี่ยของความชื้น
    totalValueH() {
      const h1 = Number(this.value_h1) || 0;
      const h2 = Number(this.value_h2) || 0;
      return (h1 + h2) / 2;
    },

    // คอมพิวเตดพรอพเพอร์ตี้เพื่อสร้างป้ายชื่อสำหรับขั้นตอนปัจจุบัน
    stepButtonLabel() {
      return `Step ${this.stepIndex + 1}`;
    },

    // คอมพิวเตดพรอพเพอร์ตี้เพื่อกำหนดสถานะของอุณหภูมิจากค่าขีดจำกัด
    temperatureStatus() {
      // ตรวจสอบว่าค่าอุณหภูมิเท่ากับค่าเฉลี่ยของขีดจำกัด
      if (this.value_temp == (this.value_t1 + this.value_t2) / 2)
        return "appropriate";
      // ตรวจสอบว่าค่าอุณหภูมิอยู่ในช่วงแต่ใกล้กับค่าด้านบน
      if (
        this.value_temp >= this.value_t1 &&
        this.value_temp <= this.value_t2 &&
        this.value_temp > (this.value_t1 + this.value_t2) / 2
      )
        return "quite hot";
      // ตรวจสอบว่าค่าอุณหภูมิอยู่ในช่วงแต่ใกล้กับค่าด้านล่าง
      if (
        this.value_temp >= this.value_t1 &&
        this.value_temp <= this.value_t2 &&
        this.value_temp < (this.value_t1 + this.value_t2) / 2
      )
        return "quite cold";
      // ตรวจสอบว่าค่าอุณหภูมิสูงกว่าขีดจำกัดด้านบน
      if (this.value_temp > this.value_t2) return "hot";
      // ตรวจสอบว่าค่าอุณหภูมิต่ำกว่าขีดจำกัดด้านล่าง
      if (this.value_temp < this.value_t1) return "cold";

      return "inappropriate"; // สถานะเริ่มต้นหากไม่ตรงกับเงื่อนไขใด ๆ
    },

    // คอมพิวเตดพรอพเพอร์ตี้เพื่อกำหนดสถานะของความชื้นจากค่าขีดจำกัด
    humidityStatus() {
      // ตรวจสอบว่าค่าความชื้นเท่ากับค่าเฉลี่ยของขีดจำกัด
      if (this.value_humi == (this.value_h1 + this.value_h2) / 2)
        return "appropriate";
      // ตรวจสอบว่าค่าความชื้นอยู่ในช่วงแต่ใกล้กับค่าด้านล่าง
      if (
        this.value_humi >= this.value_h1 &&
        this.value_humi <= this.value_h2 &&
        this.value_humi < (this.value_h1 + this.value_h2) / 2
      )
        return "quite dry";
      // ตรวจสอบว่าค่าความชื้นอยู่ในช่วงแต่ใกล้กับค่าด้านบน
      if (
        this.value_humi >= this.value_h1 &&
        this.value_humi <= this.value_h2 &&
        this.value_humi > (this.value_h1 + this.value_h2) / 2
      )
        return "quite damp";
      // ตรวจสอบว่าค่าความชื้นต่ำกว่าขีดจำกัดด้านล่าง
      if (this.value_humi < this.value_h1) return "dry";
      // ตรวจสอบว่าค่าความชื้นสูงกว่าขีดจำกัดด้านบน
      if (this.value_humi > this.value_h2) return "damp";

      return "inappropriate"; // สถานะเริ่มต้นหากไม่ตรงกับเงื่อนไขใด ๆ
    },
  },

  // ใช้สำหรับการตั้งค่าเริ่มต้นของคอมโพเนนต์. ที่นี่ทำการเชื่อมต่อกับ MQTT broker

  created() {
    this.client = mqtt.connect("ws://broker.emqx.io:8083/mqtt");
    this.client.on("connect", this.onMqttConnect.bind(this));
    this.client.on("message", this.onMqttMessage.bind(this));
  },

  // ทำการปิดการเชื่อมต่อกับ MQTT broker
  beforeDestroy() {
    this.client && this.client.end();
  },

  methods: {
    // ฟังก์ชันนี้เริ่มต้นการทำงานของระบบ
    start() {
      this.isOn = true; // เปิดระบบ
      this.value_temp = this.originalValue; // กำหนดค่าเริ่มต้นของอุณหภูมิ
      this.value_humi = this.originalValue1; // กำหนดค่าเริ่มต้นของความชื้น
      this.stepIndex = 0; // รีเซ็ตดัชนีขั้นตอนเมื่อเริ่มต้น
    },

    // ฟังก์ชันนี้หยุดการทำงานของระบบ
    stop() {
      this.isOn = false; // ปิดระบบ
      this.value_temp = 0; // รีเซ็ตค่าอุณหภูมิ
      this.value_humi = 0; // รีเซ็ตค่าความชื้น
      this.resetValues(); // รีเซ็ตค่าทั้งหมด
      this.lightStates = [false, false, false]; // ปิดไฟทั้งหมด
      this.value_t1 = 0; // รีเซ็ตค่า t1
      this.value_t2 = 0; // รีเซ็ตค่า t2
      this.value_h1 = 0; // รีเซ็ตค่า h1
      this.value_h2 = 0; // รีเซ็ตค่า h2
      this.value_state = "ถูกปิดการใช้งาน"; // ตั้งค่าสถานะเป็น "ถูกปิดการใช้งาน"
      this.systemStatus = ""; // ล้างสถานะระบบ
      this.systemStatus1 = ""; // ล้างสถานะระบบ
    },

    // ฟังก์ชันนี้รีเซ็ตค่าทั้งหมดที่ใช้ในการทำงาน
    resetValues() {
      this.value_temp = 0; // รีเซ็ตค่าอุณหภูมิ
      this.value_humi = 0; // รีเซ็ตค่าความชื้น
      this.lightStates = [false, false, false]; // ปิดไฟทั้งหมด
      this.value_t1 = 0; // รีเซ็ตค่า t1
      this.value_t2 = 0; // รีเซ็ตค่า t2
      this.value_h1 = 0; // รีเซ็ตค่า h1
      this.value_h2 = 0; // รีเซ็ตค่า h2
      this.value_state = "ถูกปิดการใช้งาน"; // ตั้งค่าสถานะเป็น "ถูกปิดการใช้งาน"
      this.systemStatus = ""; // ล้างสถานะระบบ
      this.systemStatus1 = ""; // ล้างสถานะระบบ
    },

    // ฟังก์ชันนี้จัดการการเชื่อมต่อกับ MQTT broker
    onMqttConnect() {
      this.client.publish("op", "status"); // ส่งข้อความเพื่อขอสถานะ
      this.client.subscribe("status"); // สมัครรับข้อมูลจากช่อง "status"
      this.client.subscribe("thanakarn"); // สมัครรับข้อมูลจากช่อง "thanakarn"
      this.client.subscribe("moisture"); // สมัครรับข้อมูลจากช่อง "moisture"
    },

    // ฟังก์ชันนี้จัดการข้อความที่ได้รับจาก MQTT broker
    onMqttMessage(topic, message) {
      if (topic === "status") {
        this.msg = message.toString(); // เก็บข้อความสถานะ
      }
      if (topic === "thanakarn" && this.isOn) {
        let data1 = JSON.parse(message.toString()); // แปลงข้อความ JSON เป็นออบเจกต์
        this.value_temp = parseFloat(data1.temp); // ตั้งค่าอุณหภูมิ
        this.value_humi = parseFloat(data1.humi); // ตั้งค่าความชื้น
        this.value_state = data1.state; // ตั้งค่าสถานะ
        this.value_t1 = parseFloat(data1.t1); // ตั้งค่า t1
        this.value_t2 = parseFloat(data1.t2); // ตั้งค่า t2
        this.value_h1 = parseFloat(data1.h1); // ตั้งค่า h1
        this.value_h2 = parseFloat(data1.h2); // ตั้งค่า h2
        this.updateLightStates(); // อัปเดตสถานะไฟหลังจากได้รับข้อมูล
      }
    },

    // ฟังก์ชันนี้อัปเดตสถานะของไฟตามข้อมูลที่ได้รับ
    updateLightStates() {
      const tempStatus = this.temperatureStatus; // สถานะอุณหภูมิ
      const humiStatus = this.humidityStatus; // สถานะความชื้น
      console.log(`tempStatus: ${tempStatus}, humiStatus: ${humiStatus}`); // แสดงสถานะในคอนโซล

      // ตรวจสอบสถานะและปรับสถานะไฟตามเงื่อนไขที่กำหนด
      if (tempStatus === "appropriate" && humiStatus === "appropriate") {
        this.lightStates = [false, true, false]; // เปิดไฟสีเขียวเท่านั้น
        this.systemStatus = "เหมาะสม"; // กำหนดสถานะระบบเป็น "เหมาะสม"
        this.systemStatus1 = "เหมาะสม"; // กำหนดสถานะระบบ1 เป็น "เหมาะสม"
      } else if (tempStatus === "hot" && humiStatus === "damp") {
        this.lightStates = [true, true, true]; // เปิดไฟสีแดงทั้งหมด
        this.systemStatus = "ไม่เหมาะสม"; // กำหนดสถานะระบบเป็น "ไม่เหมาะสม"
        this.systemStatus1 = "ไม่เหมาะสม"; // กำหนดสถานะระบบ1 เป็น "ไม่เหมาะสม"
      } else if (tempStatus === "cold" && humiStatus === "dry") {
        this.lightStates = [true, true, true]; // เปิดไฟสีน้ำเงินทั้งหมด
        this.systemStatus = "ไม่เหมาะสม"; // กำหนดสถานะระบบเป็น "ไม่เหมาะสม"
        this.systemStatus1 = "ไม่เหมาะสม"; // กำหนดสถานะระบบ1 เป็น "ไม่เหมาะสม"
      } else if (tempStatus === "hot" || humiStatus === "dry") {
        this.lightStates = [true, false, false]; // เปิดไฟสีแดงเท่านั้น
        this.systemStatus = "ร้อนเกินไป"; // กำหนดสถานะระบบเป็น "ร้อนเกินไป"
        this.systemStatus1 = "แห้ง"; // กำหนดสถานะระบบ1 เป็น "แห้ง"
      } else if (tempStatus === "cold" || humiStatus === "damp") {
        this.lightStates = [false, false, true]; // เปิดไฟสีน้ำเงินเท่านั้น
        this.systemStatus = "เย็นเกินไป"; // กำหนดสถานะระบบเป็น "เย็นเกินไป"
        this.systemStatus1 = "ชื้น"; // กำหนดสถานะระบบ1 เป็น "ชื้น"
      } else if (tempStatus === "quite hot" || humiStatus === "quite dry") {
        this.lightStates = [true, true, false]; // เปิดไฟสีแดงและเขียว
        this.systemStatus = "ค่อนข้างร้อน"; // กำหนดสถานะระบบเป็น "ค่อนข้างร้อน"
        this.systemStatus1 = "ค่อนข้างแห้ง"; // กำหนดสถานะระบบ1 เป็น "ค่อนข้างแห้ง"
      } else if (tempStatus === "quite cold" || humiStatus === "quite damp") {
        this.lightStates = [false, true, true]; // เปิดไฟสีน้ำเงินและเขียว
        this.systemStatus = "ค่อนข้างเย็น"; // กำหนดสถานะระบบเป็น "ค่อนข้างเย็น"
        this.systemStatus1 = "ค่อนข้างชื้น"; // กำหนดสถานะระบบ1 เป็น "ค่อนข้างชื้น"
      } else {
        this.lightStates = [true, true, true]; // เปิดไฟทั้งหมด
        this.systemStatus = "ไม่เหมาะสม"; // กำหนดสถานะระบบเป็น "ไม่เหมาะสม"
        this.systemStatus1 = "ไม่เหมาะสม"; // กำหนดสถานะระบบ1 เป็น "ไม่เหมาะสม"
      }
    },

    // ฟังก์ชันนี้หมุนเวียนค่าตัวอย่างและอัปเดตสถานะ
    cycleLights() {
      // ตรวจสอบว่าไฟเปิดอยู่หรือไม่ ถ้าไม่ให้หยุดทำงาน
      if (!this.isOn) return;

      // ข้อมูลชุดข้อมูลที่ใช้ในการปรับค่าของไฟ
      const dataSets = [
        {
          temp: "27.5",
          humi: "85.175",
          state: "1",
          t1: "27.00",
          t2: "28.00",
          h1: "80.25",
          h2: "90.10",
        },
        {
          temp: "105.00",
          humi: "3.175",
          state: "2",
          t1: "27.00",
          t2: "28.00",
          h1: "80.25",
          h2: "90.10",
        },
        {
          temp: "0.00",
          humi: "100.175",
          state: "3",
          t1: "27.00",
          t2: "28.00",
          h1: "80.25",
          h2: "90.10",
        },
        {
          temp: "28.00",
          humi: "81.175",
          state: "4",
          t1: "27.00",
          t2: "28.00",
          h1: "80.25",
          h2: "90.10",
        },
        {
          temp: "27.60",
          humi: "87.175",
          state: "5",
          t1: "27.5",
          t2: "28.00",
          h1: "80.25",
          h2: "90.10",
        },
        {
          temp: "100.60",
          humi: "100.175",
          state: "6",
          t1: "27.5",
          t2: "28.00",
          h1: "80.25",
          h2: "90.10",
        },
        // รีเซ็ตกลับไปที่ค่าตั้งต้นเมื่อกดครั้งที่ 7
      ];

      // ตรวจสอบว่า stepIndex ยังน้อยกว่าความยาวของ dataSets หรือไม่
      if (this.stepIndex < dataSets.length) {
        // ดึงข้อมูลที่ตรงกับ stepIndex จาก dataSets
        const data = dataSets[this.stepIndex];
        // อัพเดทค่าของตัวแปรต่าง ๆ ตามข้อมูลที่ได้
        this.value_temp = parseFloat(data.temp);
        this.value_humi = parseFloat(data.humi);
        this.value_state = data.state;
        this.value_t1 = parseFloat(data.t1);
        this.value_t2 = parseFloat(data.t2);
        this.value_h1 = parseFloat(data.h1);
        this.value_h2 = parseFloat(data.h2);
        // เรียกใช้ฟังก์ชันเพื่ออัพเดทสถานะของไฟ
        this.updateLightStates();
        // เพิ่มค่า stepIndex ขึ้น 1
        this.stepIndex++;
      } else {
        // ถ้า stepIndex มากกว่าหรือเท่ากับความยาวของ dataSets ให้รีเซ็ตค่า
        this.resetValues();
        // ตั้งค่า stepIndex กลับไปที่ 0
        this.stepIndex = 0;
      }
    },
    getButtonColor(index) {
      // คืนค่าสีของปุ่มขึ้นอยู่กับว่า isOn เป็นจริงหรือไม่
      return this.isOn ? "#fba0ff" : "#EEE";
    },
    getButtonLabel(index) {
      // คืนป้ายชื่อของปุ่มตามค่า index
      if (index === 6) return "Turn OFF";
      if (index === 5) return "Turn All ";
      if (index === 4) return "switch 4";
      if (index === 3) return "switch 3";
      if (index === 2) return "switch 2";
      if (index === 1) return "switch 1";
      return "";
    },
  },
};
</script>

<style scoped>
.bulb-wrapper {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.bulb {
  width: 100px;
  height: 150px;
  border-radius: 50% 50% 0 0;
  background-color: #ddd;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
}

.bulb-base {
  width: 80px;
  height: 30px;
  background-color: #888;
  border-radius: 0 0 15px 15px;
  margin-top: -10px;
}
</style>
