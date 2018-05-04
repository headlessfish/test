import queue
import threading
import socket

print_lock = threading.Lock()

# Intakes the items that I took out from the queues
def portscan(host,port):
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

        try:
                con = s.connect((target + str(host), port))
                with print_lock:
                        print('IP: {} Port{}'.format(target + str(host), port))
                con.close()
        except:
                pass
# Retrieving items from the queues
def threader():
        while True:
                        port = q.get()
                        host = h.get()
                        portscan(host,port)
                        q.task_done()
                        h.task_done()


target = "192.168.1."

q = queue.Queue()

h= queue.Queue()

# Creating 40 threads

for thread in range(40):
        t = threading.Thread(target = threader)
        t.daemon = True
        t.start()
# Creating a queue containing 1024 known ports

for port in range(1, 1024):
        q.put(port)
# Creating a range of available IPs in a /24 network
for host in range(256):
        h.put(host)

q.join()



print ("FINISH")

