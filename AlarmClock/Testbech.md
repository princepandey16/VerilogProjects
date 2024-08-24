module test;
    reg reset, clk, LD_time, LD_alarm, STOP_al, AL_ON;
    reg [1:0] H_in1;
    reg [3:0] H_in0, M_in1, M_in0;
    wire Alarm;
    wire [1:0] H_out1;
    wire [3:0] H_out0, M_out1, M_out0, S_out1, S_out0;

    aclock uut (
        .reset(reset), .clk(clk), .H_in1(H_in1), .H_in0(H_in0),
        .M_in1(M_in1), .M_in0(M_in0), .LD_time(LD_time), .LD_alarm(LD_alarm),
        .STOP_al(STOP_al), .AL_ON(AL_ON), .Alarm(Alarm),
        .H_out1(H_out1), .H_out0(H_out0), .M_out1(M_out1),
        .M_out0(M_out0), .S_out1(S_out1), .S_out0(S_out0)
    );

    initial begin
        clk = 0; forever #50 clk = ~clk;
    end

    initial begin
        reset = 1; H_in1 = 1; H_in0 = 0; M_in1 = 1; M_in0 = 4; LD_time = 0; LD_alarm = 0; STOP_al = 0; AL_ON = 0;
        #1000; reset = 0; LD_alarm = 1; AL_ON = 1;
        #1000; LD_alarm = 0;
        wait(Alarm); #1000; STOP_al = 1; #1000; STOP_al = 0; LD_time = 1;
        #1000; LD_time = 0; LD_alarm = 1; wait(Alarm); #1000; STOP_al = 1;
    end
endmodule
