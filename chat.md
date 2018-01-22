服务器端：
import java.io.*;
import java.net.*;
import java.util.*;

public class ChatServer {
    boolean started = false;
    ServerSocket ss = null;

    List<Client> clients = new ArrayList<Client>();

    public static void main(String[] args) {
        new ChatServer().start();
    }

    public void start() {
        try {
            ss = new ServerSocket(8888);
            started = true;
        } catch (BindException e) {
            System.out.println("端口使用中....");
            System.out.println("请关掉相关程序并重新运行服务器！");
            System.exit(0);
        } catch (IOException e) {
            e.printStackTrace();
        }

        try {

            while(started) {
                Socket s = ss.accept();
                Client c = new Client(s);
System.out.println("a client connected!");
                new Thread(c).start();
                clients.add(c);
                //dis.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                ss.close();
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
    }

    class Client implements Runnable {
        private Socket s;
        private DataInputStream dis = null;
        private DataOutputStream dos = null;
        private boolean bConnected = false;

        public Client(Socket s) {
            this.s = s;
            try {
                dis = new DataInputStream(s.getInputStream());
                dos = new DataOutputStream(s.getOutputStream());
                bConnected = true;
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        public void send(String str) {
            try {
                dos.writeUTF(str);
            } catch (IOException e) {
                clients.remove(this);
                System.out.println("对方退出了！我从List里面去掉了！");
                //e.printStackTrace();
            }
        }

        public void run() {
            try {
                while(bConnected) {
                    String str = dis.readUTF();
System.out.println(str);
                    for(int i=0; i<clients.size(); i++) {
                        Client c = clients.get(i);
                        c.send(str);
//System.out.println(" a string send !");
                    }
                    /*
                    for(Iterator<Client> it = clients.iterator(); it.hasNext(); ) {
                        Client c = it.next();
                        c.send(str);
                    }
                    */
                    /*
                    Iterator<Client> it = clients.iterator();
                    while(it.hasNext()) {
                        Client c = it.next();
                        c.send(str);
                    }
                    */
                }
            } catch (EOFException e) {
                System.out.println("Client closed!");
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                try {
                    if(dis != null) dis.close();
                    if(dos != null) dos.close();
                    if(s != null)  {
                        s.close();
                        //s = null;
                    }

                } catch (IOException e1) {
                    e1.printStackTrace();
                }


            }
        }

    }
}
客户端：
import java.awt.*;
import java.awt.event.*;
import java.io.*;
import java.net.*;

public class ChatClient extends Frame {
    Socket s = null;
    DataOutputStream dos = null;
    DataInputStream dis = null;
    private boolean bConnected = false;

    TextField tfTxt = new TextField();

    TextArea taContent = new TextArea();

    Thread tRecv = new Thread(new RecvThread()); 

    public static void main(String[] args) {
        new ChatClient().launchFrame(); 
    }

    public void launchFrame() {
        setLocation(400, 300);
        this.setSize(300, 300);
        add(tfTxt, BorderLayout.SOUTH);
        add(taContent, BorderLayout.NORTH);
        pack();
        this.addWindowListener(new WindowAdapter() {

            @Override
            public void windowClosing(WindowEvent arg0) {
                disconnect();
                System.exit(0);
            }

        });
        tfTxt.addActionListener(new TFListener());
        setVisible(true);
        connect();

        tRecv.start();
    }

    public void connect() {
        try {
            s = new Socket("127.0.0.1", 8888);
            dos = new DataOutputStream(s.getOutputStream());
            dis = new DataInputStream(s.getInputStream());
System.out.println("connected!");
            bConnected = true;
        } catch (UnknownHostException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    public void disconnect() {
        try {
            dos.close();
            dis.close();
            s.close();
        } catch (IOException e) {
            e.printStackTrace();
        }

        /*
        try {
            bConnected = false;
            tRecv.join();
        } catch(InterruptedException e) {
            e.printStackTrace();
        } finally {
            try {
                dos.close();
                dis.close();
                s.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        */
    }

    private class TFListener implements ActionListener {

        public void actionPerformed(ActionEvent e) {
            String str = tfTxt.getText().trim();
            //taContent.setText(str);
            tfTxt.setText("");

            try {
//System.out.println(s);
                dos.writeUTF(str);
                dos.flush();
                //dos.close();
            } catch (IOException e1) {
                e1.printStackTrace();
            }

        }

    }

    private class RecvThread implements Runnable {

        public void run() {
            try {
                while(bConnected) {
                    String str = dis.readUTF();
                    //System.out.println(str);
                    taContent.setText(taContent.getText() + str + '\n');
                }
            } catch (SocketException e) {
                System.out.println("退出了，bye!");
            } catch (EOFException e) {
                System.out.println("推出了，bye - bye!");
            } catch (IOException e) {
                e.printStackTrace();
            } 

        }

    }
}













客户端和客户端聊天：
server端：
package com.javacodegeeks.android.androidsocketserver;
 
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.ServerSocket;
import java.net.Socket;
import android.app.Activity;
import android.os.Bundle;
import android.os.Handler;
import android.util.Log;
import android.widget.TextView;
 
public class Server extends Activity
{
    private ServerSocket serverSocket;
    private Handler updateConversationHandler;
    private Thread serverThread = null;
    private TextView text;
    public static final int SERVERPORT = 6000;
 
    @Override
    public void onCreate(Bundle savedInstanceState)
    {
 
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
 
        text = (TextView) findViewById(R.id.text2);
 
        updateConversationHandler = new Handler();
 
        this.serverThread = new Thread(new ServerThread());
        this.serverThread.start();
 
    }
 
    @Override
    protected void onStop()
    {
        super.onStop();
        try
        {
            serverSocket.close();
        }
        catch (IOException e)
        {
            e.printStackTrace();
        }
    }
 
    private class ServerThread implements Runnable
    {
 
        public void run()
        {
            Socket socket = null;
            try
            {
                serverSocket = new ServerSocket(SERVERPORT);
            }
            catch (IOException e)
            {
                e.printStackTrace();
            }
            while (!Thread.currentThread().isInterrupted())
            {
                try
                {
                    socket = serverSocket.accept(); // TODO
 
                    CommunicationThread commThread = new CommunicationThread(socket);
                    new Thread(commThread).start();
                }
                catch (IOException e)
                {
                    Log.i("liu", "socket.accept()失败");
 
                    e.printStackTrace();
                }
            }
        }
    }
 
    private class CommunicationThread implements Runnable
    {
        private Socket clientSocket;
        private BufferedReader input;
 
        public CommunicationThread(Socket clientSocket)
        {
            this.clientSocket = clientSocket;
 
            Log.i("liu", "获取到了client的Socket");
 
            try
            {
                this.input = new BufferedReader(new InputStreamReader(this.clientSocket.getInputStream())); // TODO
            }
            catch (IOException e)
            {
                Log.i("liu", "input获取失败");
                e.printStackTrace();
            }
        }
 
        public void run()
        {
            while (!Thread.currentThread().isInterrupted())
            {
                try
                {
                    String read = input.readLine(); // TODO
                    Log.i("liu", read);
                    updateConversationHandler.post(new updateUIThread(read));
                }
                catch (IOException e)
                {
                    Log.i("liu", "input读取失败");
                    e.printStackTrace();
                }
            }
        }
    }
 
    private class updateUIThread implements Runnable
    {
        private String msg;
 
        public updateUIThread(String str)
        {
            this.msg = str;
        }
 
        @Override
        public void run()
        {
            text.setText(text.getText().toString() + "Client Says: " + msg + "\n");
        }
 
    }
 
}



client端：
package com.javacodegeeks.android.androidsocketclient;
 
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.OutputStreamWriter;
import java.io.PrintWriter;
import java.net.InetAddress;
import java.net.Socket;
import java.net.UnknownHostException;
import android.app.Activity;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.EditText;
 
public class Client extends Activity
{
    private Socket socket;
    private static final int SERVERPORT = 4000;
    private static final String SERVER_IP = "10.0.2.2";
 
    @Override
    public void onCreate(Bundle savedInstanceState)
    {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
 
        new Thread(new ClientThread()).start();
    }
 
    public void onClick(View view)
    {
        try
        {
            EditText et = (EditText) findViewById(R.id.EditText01);
            String str = et.getText().toString();
 
            Log.i("liu", "点击按钮");
 
            PrintWriter out = new PrintWriter(new BufferedWriter(new OutputStreamWriter(socket.getOutputStream())), true); // TODO
            out.println(str);
        }
        catch (Exception e)
        {
            Log.i("liu", "write失败");
            e.printStackTrace();
        }
    }
 
    class ClientThread implements Runnable
    {
 
        @Override
        public void run()
        {
            try
            {
                InetAddress serverAddr = InetAddress.getByName(SERVER_IP);
                socket = new Socket(serverAddr, SERVERPORT);
            }
            catch (UnknownHostException e1)
            {
                e1.printStackTrace();
            }
            catch (IOException e1)
            {
                e1.printStackTrace();
            }
        }
    }
}
