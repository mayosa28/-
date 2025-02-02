import { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";
import { Table, TableBody, TableCell, TableHead, TableHeader, TableRow } from "@/components/ui/table";

const ACCESS_PASSWORD = "M2025";

export default function WeddingBookingApp() {
  const [bookings, setBookings] = useState([]);
  const [formData, setFormData] = useState({
    date: "",
    time: "",
    totalAmount: "",
    advancePayment: "",
    discount: "",
    eventType: "",
    details: "",
  });
  const [password, setPassword] = useState("");
  const [authenticated, setAuthenticated] = useState(false);

  const handleInputChange = (e) => {
    setFormData({ ...formData, [e.target.name]: e.target.value });
  };

  const handlePasswordSubmit = () => {
    if (password === ACCESS_PASSWORD) {
      setAuthenticated(true);
    } else {
      alert("Incorrect password. Please try again.");
    }
  };

  const calculateRemaining = (total, advance, discount) => {
    return total - advance - discount;
  };

  const addBooking = () => {
    const remainingAmount = calculateRemaining(
      parseFloat(formData.totalAmount || 0),
      parseFloat(formData.advancePayment || 0),
      parseFloat(formData.discount || 0)
    );

    setBookings([
      ...bookings,
      { ...formData, remainingAmount: remainingAmount.toFixed(2) },
    ]);
    setFormData({
      date: "",
      time: "",
      totalAmount: "",
      advancePayment: "",
      discount: "",
      eventType: "",
      details: "",
    });
  };

  if (!authenticated) {
    return (
      <div className="p-8">
        <h1 className="text-2xl font-bold mb-4">Enter Password</h1>
        <Input
          type="password"
          placeholder="Enter access password"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
        />
        <Button className="mt-4" onClick={handlePasswordSubmit}>
          Submit
        </Button>
      </div>
    );
  }

  return (
    <div className="p-8">
      <h1 className="text-2xl font-bold mb-4">Wedding Venue Booking App</h1>
      <Card className="mb-8 p-4">
        <CardContent>
          <div className="grid grid-cols-2 gap-4">
            <div>
              <Label htmlFor="date">Date</Label>
              <Input
                type="date"
                name="date"
                value={formData.date}
                onChange={handleInputChange}
              />
            </div>
            <div>
              <Label htmlFor="time">Time</Label>
              <Input
                type="time"
                name="time"
                value={formData.time}
                onChange={handleInputChange}
              />
            </div>
            <div>
              <Label htmlFor="eventType">Event Type</Label>
              <Input
                type="text"
                name="eventType"
                value={formData.eventType}
                onChange={handleInputChange}
              />
            </div>
            <div>
              <Label htmlFor="totalAmount">Total Amount</Label>
              <Input
                type="number"
                name="totalAmount"
                value={formData.totalAmount}
                onChange={handleInputChange}
              />
            </div>
            <div>
              <Label htmlFor="advancePayment">Advance Payment</Label>
              <Input
                type="number"
                name="advancePayment"
                value={formData.advancePayment}
                onChange={handleInputChange}
              />
            </div>
            <div>
              <Label htmlFor="discount">Discount</Label>
              <Input
                type="number"
                name="discount"
                value={formData.discount}
                onChange={handleInputChange}
              />
            </div>
            <div className="col-span-2">
              <Label htmlFor="details">Event Details</Label>
              <Input
                type="text"
                name="details"
                value={formData.details}
                onChange={handleInputChange}
              />
            </div>
          </div>
          <Button className="mt-4" onClick={addBooking}>
            Add Booking
          </Button>
        </CardContent>
      </Card>
      <h2 className="text-xl font-bold mb-2">Bookings</h2>
      <Table>
        <TableHeader>
          <TableRow>
            <TableHead>Date</TableHead>
            <TableHead>Time</TableHead>
            <TableHead>Event Type</TableHead>
            <TableHead>Total Amount</TableHead>
            <TableHead>Advance</TableHead>
            <TableHead>Discount</TableHead>
            <TableHead>Remaining</TableHead>
            <TableHead>Details</TableHead>
          </TableRow>
        </TableHeader>
        <TableBody>
          {bookings.map((booking, index) => (
            <TableRow key={index}>
              <TableCell>{booking.date}</TableCell>
              <TableCell>{booking.time}</TableCell>
              <TableCell>{booking.eventType}</TableCell>
              <TableCell>{booking.totalAmount}</TableCell>
              <TableCell>{booking.advancePayment}</TableCell>
              <TableCell>{booking.discount}</TableCell>
              <TableCell>{booking.remainingAmount}</TableCell>
              <TableCell>{booking.details}</TableCell>
            </TableRow>
          ))}
        </TableBody>
      </Table>
    </div>
  );
}
