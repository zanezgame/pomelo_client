# pomelo_client
pomelo client for egret navtie

1，pomelo client 前端库支持 egret 原生打包

2，修复 收不到 pomelo.reqId ==128 收到 返回BUG

使用方法  typescript
 
 初始化pomelo
 
 private initSocket() {

        if (this.socket == null) {
            this.socket = new Pomelo();
            var self = this;

            this.socket.on('server_push_message', (msg) => {
                if (self.routeMap[msg.route]) {
                    var cb: Function = self.routeMap[msg.route].cb;
                    var thisArg: any = self.routeMap[msg.route].thisArg;
                    cb.apply(thisArg, [msg.body]);
                }
            });


            this.socket.on('onKick', (msg) => {

                self.kick = true;
                AlertMgr_New.inst.alert(LangConstans.common_tips_text, LangConstans.account_relogin_text, () => {

                    setTimeout(function () {
                        self.kick = false;
                        self.disconnect();
                        self.connectTime = 0;
                        game.AppFacade.getInstance().sendNotification(NetNotify.CONNECT);
                    }, 1000);

                }, [], null, [], self);
            });



            this.socket.on('heartbeat_timeout', () => {

                AlertMgr_New.inst.alert(LangConstans.common_tips_text, LangConstans.account_relogin_text, () => {

                    setTimeout(function () {
                        self.kick = false;
                        self.disconnect();
                        self.connectTime = 0;
                        game.AppFacade.getInstance().sendNotification(NetNotify.CONNECT);
                    }, 1000);

                }, [], null, [], self);
            })

        }

    }
 

请求服务request

public request(route: string, msg: any, cb: Function, thisArg?: any): void {
        this.socket.request(route, msg, (response) => {
            cb.call(thisArg, response);
        });
    }
    
    

请求服务 notify  
    public notify(route: string, msg: any): void {
        this.socket.notify(route, msg);
    }
