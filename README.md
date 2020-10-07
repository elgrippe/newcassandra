# newcassandra
updating linux vms with data dirs and such

set up ssh keys between a lot of servers using ansible:

ansible all -m shell -a "ssh-keygen -q -b 2048 -t rsa -N '' -C 'creating SSH' -f ~/.ssh/id_rsa creates='~/.ssh/id_rsa'" -i inventory -b --become-user=root

ansible all -m fetch -a "src='~/.ssh/id_rsa.pub' dest='buffer/{{inventory_hostname}}-id_rsa.pub' flat='yes'" -i inventory -b --become-user=root

for i in `grep at4p inventory`; do ansible all -m authorized_key -a "user='root' state='present' key='{{ lookup('file','buffer/${i}-id_rsa.pub')}}'" -i inventory -b --become-user=root; done
