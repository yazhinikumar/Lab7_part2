# Lab7_part2
// the librariers that we used in this code
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Net;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace lab7_try2
{
    public partial class Form1 : Form
    {
        int starflag = 0;
        int flag_sensor;
        string RxString;
        string temp = "30";
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            serialPort1.Close();
         
            serialPort1.PortName = "COM4";
            serialPort1.BaudRate = 9600;

            serialPort1.Open();
             if(serialPort1.IsOpen)
            {
                // startSerial.Enabled = false;
                // serialStop.Enabled = true;
                textBox1.ReadOnly = false;
            }
        }

        private void button2_Click(object sender, EventArgs e)
        {
            
            serialPort1.Close();
             
            
                // startSerial.Enabled = true;
                // serialStop.Enabled = false;
                textBox1.ReadOnly = true;
            
        }

        private void label1_Click(object sender, EventArgs e)
        {
            //textBox1.AppendText(RxString);
        }

        private void label2_Click(object sender, EventArgs e)
        {
            //   textBox2.AppendText(RxString);
        }

        private void button3_Click(object sender, EventArgs e)
        {
            WebClient client = new WebClient();
            client.DownloadString("http://api.thingspeak.com/channels/1595755/feed.json");
            label6.Text = client.DownloadString("http://api.thingspeak.com/channels/1595755/field/field1/last.text");
        }

        private void label5_Click(object sender, EventArgs e)
        {
            textBox2.AppendText(RxString);
        }

        private void button4_Click(object sender, EventArgs e)
        {

        }

        private void label6_Click(object sender, EventArgs e)
        {
            textBox1.AppendText(RxString);
        }

        private void label3_Click(object sender, EventArgs e)
        {

        }

        private void textBox1_TextChanged(object sender, EventArgs e)
        {

        }

        private void label4_Click(object sender, EventArgs e)
        {

        }

        private void textBox2_TextChanged(object sender, EventArgs e)
        {

        }

        private void timer1_Tick(object sender, EventArgs e)
        {
            if (!string.Equals(textBox1.Text, ""))
            {
                if (serialPort1.IsOpen) serialPort1.Close();
                try
                {
                    /* if (RxString[0] == 'B')
                     * {
                     *      flag_sensor = 10;
                     *  }*/

                    const string WRITEKEY = "02EU4M8VOBLWD8U6" ;
                    string strUpdateBase = "http://api.thingspeak.com/update";

                    string strUpdateURI = strUpdateBase + "?api_key=" + WRITEKEY;
                    string strField1 = textBox1.Text;

                    HttpWebRequest ThingsSpeakReq;
                    HttpWebResponse ThingsSpeakResp;
                    strUpdateURI += "&field1=" + strField1;

                    flag_sensor++;
                    ThingsSpeakReq = (HttpWebRequest)WebRequest.Create(strUpdateURI);

                    ThingsSpeakResp = (HttpWebResponse)ThingsSpeakReq.GetResponse();
                    ThingsSpeakResp.Close();

                    if (!(string.Equals(ThingsSpeakResp.StatusDescription, "OK")))
                    {
                        Exception exData = new Exception(ThingsSpeakResp.StatusDescription);
                        throw exData;
                    }
                }
                catch (Exception ex)
                {

                }
                textBox1.Text = "";

                serialPort1.Open();
            }
        }
        private void Form1_Load(object sender, EventArgs e)
        {
            if (serialPort1.IsOpen)
                serialPort1.Close();

            serialPort1.PortName = "COM4";
            serialPort1.BaudRate = 9600;

        }
    }
}
