import { LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, ResponsiveContainer } from 'recharts';
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from "@/components/ui/card";
import { format } from 'date-fns';

interface MoodEntry {
  value: number;
  label: string;
  emoji: string;
  timestamp: Date;
}

interface MoodChartProps {
  moodHistory: MoodEntry[];
}

export function MoodChart({ moodHistory }: MoodChartProps) {
  const chartData = moodHistory.slice(-7).map((entry, index) => ({
    date: format(entry.timestamp, 'MMM dd'),
    mood: entry.value,
    label: entry.label,
    emoji: entry.emoji,
  }));

  const averageMood = moodHistory.length > 0 
    ? (moodHistory.reduce((sum, entry) => sum + entry.value, 0) / moodHistory.length).toFixed(1)
    : '0';

  return (
    <Card className="shadow-calm">
      <CardHeader>
        <CardTitle className="flex items-center justify-between">
          Mood Trends
          <span className="text-sm font-normal text-muted-foreground">
            7-day view
          </span>
        </CardTitle>
        <CardDescription>
          Track your emotional wellness over time • Average: {averageMood}/5
        </CardDescription>
      </CardHeader>
      <CardContent>
        {moodHistory.length > 0 ? (
          <div className="h-64">
            <ResponsiveContainer width="100%" height="100%">
              <LineChart data={chartData}>
                <CartesianGrid strokeDasharray="3 3" className="opacity-30" />
                <XAxis 
                  dataKey="date" 
                  axisLine={false}
                  tickLine={false}
                  className="text-sm"
                />
                <YAxis 
                  domain={[1, 5]}
                  axisLine={false}
                  tickLine={false}
                  className="text-sm"
                />
                <Tooltip 
                  content={({ active, payload, label }) => {
                    if (active && payload && payload[0]) {
                      const data = payload[0].payload;
                      return (
                        <div className="bg-white p-3 rounded-lg shadow-lg border">
                          <p className="font-medium">{label}</p>
                          <p className="text-sm text-muted-foreground">
                            {data.emoji} {data.label} ({data.mood}/5)
                          </p>
                        </div>
                      );
                    }
                    return null;
                  }}
                />
                <Line 
                  type="monotone" 
                  dataKey="mood" 
                  stroke="hsl(var(--primary))"
                  strokeWidth={3}
                  dot={{ fill: "hsl(var(--primary))", strokeWidth: 2, r: 6 }}
                  activeDot={{ r: 8, fill: "hsl(var(--primary-glow))" }}
                />
              </LineChart>
            </ResponsiveContainer>
          </div>
        ) : (
          <div className="h-64 flex items-center justify-center text-muted-foreground">
            <div className="text-center">
              <div className="text-4xl mb-2">📊</div>
              <p>Start tracking your mood to see trends here</p>
            </div>
          </div>
        )}
      </CardContent>
    </Card>
  );
}
